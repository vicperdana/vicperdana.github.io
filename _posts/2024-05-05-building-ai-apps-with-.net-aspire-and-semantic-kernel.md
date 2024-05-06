---
title: "Build AI Apps the Easy Way with Semantic Kernel and .NET Aspire"
excerpt_separator: "<!--more-->"
layout: single
toc: true
classes: wide
categories:
  - ai
tags:
  - ai
---

Hello, Cloud Native and AI enthusiasts! Today, we will delve into the fascinating world of AI applications. Specifically, we will explore how we can build AI applications with ease using the Semantic Kernel and .NET Aspire. So, if you're excited about creating intelligent, scalable, and efficient apps, keep reading.

## Understanding Semantic Kernel and .NET Aspire 🤖

[Semantic Kernel (SK)](https://github.com/microsoft/semantic-kernel) is an open source framework that lets you add agents that can interact with your application. It empowers developers to build advanced AI applications using AI agents. An AI agent in Semantic Kernel comprises plugins, planners, personas, and a kernel at its core. The kernel holds the crucial role of orchestrating your code with AI, while plugins confer skills on your agent to interact with real-world entities - think of the limitless world of APIs.

Additionally, [.NET Aspire](https://github.com/dotnet/aspire) is a .NET Foundation project aimed at enhancing the development experience when constructing complex cloud-native applications. It offers developers building blocks to construct resilient applications and has components designed for seamless integration as developers build their apps.

Together, Semantic Kernel and .NET Aspire provide a powerful combination for building AI applications. With Semantic Kernel, you can create intelligent agents that can interact with your application, while .NET Aspire offers a robust framework for building cloud-native applications. The integration of these two tools enables developers to build AI applications that are scalable, efficient, and intelligent.

## Building an Intelligent Assistant: The Breakdown 🏛️
To illustrate the power and flexibility of Semantic Kernel and .NET Aspire, we will build an intelligent assistant in the form of a chat service for an e-commerce website. This assistant will provide instant responses to user queries in a contextually aware manner, significantly enhancing the customer experience on the website.

We're going to use a sample e-commerce website, the Aspire Shop, and add a chat service to it. The chat service will be powered by Azure OpenAI and will interact with the website's catalog service, providing users with relevant product suggestions based on their queries.
![Screenshot of the architecture](/assets/images/architecture.gif)

## Implementing the Chat Service: A Step-by-Step Guide 🛠️
The first step in implementing the chat service is adding the necessary dependencies in our application. These dependencies include the OpenAI extensions, the chat completion service, and the catalog service, which provides access to the product catalog of the e-commerce website.

Go ahead and create a new project for the chat service. 


First, we create our plugin. In Semantic Kernel, a plugin is a set of functions that an AI agent can execute. For our intelligent assistant, our plugin will fetch catalog items based on the user's query and present them to the user.

```csharp
namespace AspireShop.ChatService.Plugins;

[Description("Filter catalog items")]
public class FilterCatalogItem(CatalogChatClient catalogChatClient)
{
  [KernelFunction, Description("Return a list of catalog items filtered by name or description")]
    
  public async Task<CatalogItemsPage?> GetCatalogItems(
        [Description("Name or description of the item to be queried")]
        string? searchText)
    {
        if (catalogChatClient is not null)
        {
            var items = await catalogChatClient.SearchItemsAsync(searchText);
            return items;
        }
        else
        {
            throw new InvalidOperationException("CatalogServiceClient is not available");
        }
    }
}
```

Once the plugin is added, we can start using Dependency Injection for separation of concerns. Check out also the [Dependency Injection blog post](https://devblogs.microsoft.com/semantic-kernel/using-semantic-kernel-with-dependency-injection/) from the Semantic Kernel team for more information.




```csharp
//Add Semantic Kernel Services using Azure OpenAI
builder.Services.AddOptions<AzureOpenAI>()
    .Bind(builder.Configuration.GetSection(nameof(AzureOpenAI)))
    .ValidateDataAnnotations()
    .ValidateOnStart();

// Chat completion service that kernels will use
builder.Services.AddSingleton<IChatCompletionService>(sp =>
{
    AzureOpenAI options = sp.GetRequiredService<IOptions<AzureOpenAI>>().Value;
    return new AzureOpenAIChatCompletionService(options.ChatDeploymentName, options.Endpoint, options.ApiKey);
});

builder.Services.AddKeyedSingleton<FilterCatalogItem>("FilterCatalogItem", (Func<IServiceProvider, object, FilterCatalogItem>) ((sp, key) =>
{
    var catalogClientChatService = sp.GetRequiredService<CatalogChatClient>();
    if (catalogClientChatService is null)
    {
        throw new InvalidOperationException("CatalogChatClient is not registered in the service provider.");
    }
    return new FilterCatalogItem(catalogClientChatService);
}));

builder.Services.AddKeyedTransient<Kernel>("AspireShopKernel", (sp, key) =>
{
    // Create a collection of plugins that the kernel will use
    KernelPluginCollection pluginCollection = [];
    pluginCollection.AddFromObject(sp.GetRequiredKeyedService<FilterCatalogItem>("FilterCatalogItem"), "FilterCatalogItem");
    #pragma warning disable SKEXP0050
    pluginCollection.AddFromType<ConversationSummaryPlugin>();
    // When created by the dependency injection container, Semantic Kernel logging is included by default
    return new Kernel(sp, pluginCollection);
});
```

Once our plugin is ready, we move on to create our chat service. This service will handle user queries, process them using the AI agent and return relevant responses. We will use Azure OpenAI to power our chat service, enabling it to provide intelligent responses to user queries.  Here's a snippet of the relevant code.

```csharp
      string systemPrompt =
            """
            You are an AI assistant that helps people find information from Aspire Shop. You can help users find products, get information about product and nothing else. Do not offer to buy products, add to cart, or any other actions. If you found catalog items, only mention the name and the price and not any other information. Do not say that there is no picture available or not as it is not relevant. If the user asks for items in plural, remove the 's' and search for the singular form.
            """;
             
        history.AddSystemMessage(systemPrompt);
        history.AddUserMessage(message);
        var chatCompletionService = _kernel.GetRequiredService<IChatCompletionService>();
        var result = await chatCompletionService.GetChatMessageContentAsync(history,
            executionSettings: openAIPromptExecutionSettings, kernel: _kernel);
        history.AddAssistantMessage(result.Content!);
        CatalogItemsPage? catalogItemsPage = null;
        foreach (var chatHistoryItem in history)
        {
            if (chatHistoryItem.Role == AuthorRole.Tool)
            {
                if (chatHistoryItem.Content != null)
                    catalogItemsPage = JsonSerializer.Deserialize<CatalogItemsPage>(chatHistoryItem.Content);
            }
        }
        var chatServiceResult = new ChatService(result.Content ?? string.Empty, history, catalogItemsPage?.SearchText);
        return chatServiceResult;
```

We then integrate the chat service into the Aspire project, adding a new controller to handle the API chat endpoint and ensuring seamless interaction between the chat service and the rest of the Aspire application.

```csharp
var builder = DistributedApplication.CreateBuilder(args);

// Add Azure Chat Service with Azure OpenAI
var chatDeploymentName = builder.AddParameter("chatDeploymentName", secret: true);
var chatEndpoint = builder.AddParameter("chatEndpoint", secret: true);
var chatApiKey = builder.AddParameter("chatApiKey", secret: true);
var chatService = builder.AddProject<Projects.AspireShop_ChatService>("chatservice")
    .WithEnvironment("AzureOpenAI__ChatDeploymentName", chatDeploymentName)
    .WithEnvironment("AzureOpenAI__Endpoint", chatEndpoint)
    .WithEnvironment("AzureOpenAI__ApiKey", chatApiKey)
    .WithReference(catalogService)
    .WithReference(postgres);


builder.AddProject<Projects.AspireShop_Frontend>("frontend")
    .WithReference(basketService)
    .WithReference(catalogService)
    .WithReference(chatService)
    .WithExternalHttpEndpoints();
...
```

## Deploying the Application: Bring Your Intelligent Assistant to Life! 🚀🚀🚀
After successfully integrating the chat service into the Aspire project, the next step is deploying our application. We have two options for deploying our application to Azure: the Azure Developer CLI or GitHub Actions. Both offer seamless deployment and allow us to get our intelligent assistant up and running.

Once deployed, we can test our chat service. By sending queries through the website's chat box, we can observe the assistant's responses. This allows us to verify the functionality of the chat service and ensure that it is providing relevant and accurate responses.

![Screenshot of the complete Aspire Shop frontend](/assets/images/aspireshop-frontend-complete.png)

## Future Improvements and Considerations

While our chat service is now functional and enhancing the user experience on the website, there are always opportunities for improvement and optimization.

One way we could enhance our intelligent assistant is by implementing a retrieval augmented generation strategy. This would involve using an embedding-based indexing strategy to retrieve relevant catalog items, enabling faster and more accurate responses to user queries.

Another potential area of improvement is in terms of error handling and scalability. As our application grows and attracts more users, we need to ensure that it can scale efficiently to handle increased demand.

## Wrapping Up

Building AI applications is now more approachable with tools like Semantic Kernel and .NET Aspire, which ease the development process and ensure the robustness needed for modern AI applications. Our chat service demonstrates just a fraction of what these tools can do, from managing complex tasks to offering personalized user experiences.

As these frameworks evolve, new features such as improved data indexing in Semantic Kernel and enhanced developer tooling in .NET Aspire are on the horizon, promising to further simplify AI development. These advancements are making these tools indispensable for developers, especially in marrying up with the versatile nature of cloud-native characteristics.

For reference, check out the [Project links](#further-resources) below including my recent Reactor presentation. Keep exploring and learning in this ever-evolving field!

### Further Resources
- [AspireShopWithSemanticKernel](https://github.com/vicperdana/AspireShopWithSemanticKernel)
- See the presentation [slides](https://github.com/vicperdana/AspireShopWithSemanticKernel/blob/main/assets/Reactor.pdf) and the video below for more information on the project.<br/>
  [![Build AI Apps the easy way using the Semantic Kernel SDK](/assets/images/reactortalk.png)](https://www.youtube.com/watch?v=7xOAc_twiAQ)


