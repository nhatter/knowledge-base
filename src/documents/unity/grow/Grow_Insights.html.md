---
layout: "content"
image: "Tutorial"
title: "Soomla Insights"
text: "Get started with Soomla Insights for Unity. Here you can find initialization instructions, event handling and usage examples."
position: 4
theme: 'platforms'
collection: 'unity_grow'
module: 'grow'
platform: 'unity'
---

# Soomla Insights

## Overview

Soomla Insights brings you priceless insights about your users. You can use the provided insights to take actions on your users at real time during gameplay or when your users arrive in the game. Things you can do may include:  


- Create special prices for paying users in your genre.
- Adapt the game difficulty for the specific user.
- Create push campaigns.
- A lot more ... you can create your own personalization features and even share them with others on http://answers.soom.la.

Currently, Insights supports PayInsights which categorizes users according to their pay-rank. More about this below.

## Integration

<div class="info-box">Soomla-Insights depends on Soomla-Highway, so make sure you follow the [Setup GROW](/unity/grow/grow_gettingstarted/#SetupGROW) instructions before integrating Soomla-Insights.</div>

1. Initialize `SoomlaInsights` **after** initializing `SoomlaHighway`:

    ``` cs
    SoomlaInsights.Initialize();
    ```

2. Create event handler functions in order to be notified about (and handle) Soomla-Insights related events. See [Events](/unity/grow/Grow_Insights/#Events) for more information.

3. Once initialized, Soomla-Insights will automatically retrieve relevant insights from the server. Once the insights are ready (see [`OnInsightsRefreshFinished`](/unity/grow/Grow_Insights/#OnInsightsRefreshFinished)) you can access them as explained below.

## Events

Following is a list of all the events in Soomla-Insights and an example of how to observe & handle them.

### OnInsightsInitialized

This event is triggered when the Soomla-Insights feature is initialized and ready.

``` cs
HighwayEvents.OnInsightsInitialized += onInsightsInitialized;

public void onInsightsInitialized() {
// ... your game specific implementation here ...
}
```

### OnInsightsRefreshStarted

This event is triggered when fetching insights from the server has started.

``` cs
HighwayEvents.OnInsightsRefreshStarted += onInsightsRefreshStarted;

public void onInsightsRefreshStarted() {
// ... your game specific implementation here ...
}
```

### OnInsightsRefreshFinished

This event is triggered when fetching insights from the server has finished.

``` cs
HighwayEvents.OnInsightsRefreshFinished += onInsightsRefreshFinished;

public void onInsightsRefreshFinished() {
// ... your game specific implementation here ...
}
```

### OnInsightsRefreshFailed

This event is triggered when fetching insights from the server has failed.

``` cs
HighwayEvents.OnInsightsRefreshFailed += onInsightsRefreshFailed;

public void onInsightsRefreshFailed() {
// ... your game specific implementation here ...
}
```

## Main Classes & Methods

Here you can find descriptions of the main classes of Soomla-Insights. These classes contain functionality for insights-related operations such as refreshing insights, retrieving and using them.

### SoomlaInsights

`SoomlaInsights` is the main class of Soomla-Insights which is in charge of fetching insights.

#### Functions

**`Initialize()`**

Initializes the Soomla-Insights feature. Once initialized, the `OnInsightsInitialized` event is triggered.

**`RefreshInsights()`**

Manually refresh the insights. The `OnInsightsRefreshStarted` event is triggered once the refresh process is started, and one of `OnInsightsRefreshFinished` or `OnInsightsRefreshFailed` is triggered depending on the refresh outcome.

#### Members

**`UserInsights`**

The [User-Insights](/unity/grow/Grow_Insights/#UserInsights) received from the server.

<div class="info-box">Soomla-Insights caches its data on the device so that it's accessible even when there is no internet connection.</div>

### UserInsights

`UserInsights` holds insights related to the user currently playing the game.
Located in `SoomlaInsights` and can be accessed using `SoomlaInsights.UserInsights`.

#### Members

**`PayInsights`**

The [Pay-Insights](/unity/grow/Grow_Insights/#PayInsights) received from the server.

### PayInsights

`PayInsights` holds insights related to the user's payments.
Located in `UserInsights` and can be accessed using `SoomlaInsights.UserInsights.PayInsights`.

#### Members

**`PayRankByGenre`**

A `Dictionary` providing the user's pay-rank by [Genre](/unity/grow/Grow_Insights/#Genre)

##### Possible return values

- -1: No insights for selected genre
- 0: The user has paid 0$ in total
- 1: The user has paid up to 1$
- 2: The user has paid up to 5$
- 3: The user has paid up to 10$
- 4: The user has paid up to 50$
- 5: The user has paid up to 100$
- 6: The user has paid more than 100$

<div class="info-box">NOTE: Pay rank is calculated according to the user's total revenue from ALL games using Soomla.</div>

### Genre

`Genre` represents a game genre.

#### Usage

For example, in order to access a user's pay rank by the `Action` genre use `SoomlaInsights.UserInsights.PayInsights.PayRankByGenre[Genre.Action]`

### Example

``` cs

void Start () {

    // Add event listeners - Make sure to set the event handlers before you initialize
    HighwayEvents.OnInsightsInitialized += OnSoomlaInsightsInitialized;
    HighwayEvents.OnInsightsRefreshFinished += OnSoomlaInsightsRefreshFinished;

    // Initialize SoomlaHighway
    SoomlaHighway.Initialize();

    // Initialize SoomlaInsights
    SoomlaInsights.Initialize();

}

void OnSoomlaInsightsInitialized () {
    Debug.Log("Soomla insights has been initialized.");
}

void OnSoomlaInsightsRefreshFinished (){
    if (SoomlaInsights.UserInsights.PayInsights.PayRankByGenre[Genre.Educational] > 3) {
        // ... Do stuff according to your business plan ...
    }
}


```
