# ProxyCrawl

.NET library for scraping and crawling websites using the ProxyCrawl API.

## Installation

See [nuget package](https://www.nuget.org/packages/ProxyCrawlAPI/)

## Crawling API Usage

Initialize the API with one of your account tokens, either normal or javascript token. Then make get or post requests accordingly.

You can get a token for free by [creating a ProxyCrawl account](https://proxycrawl.com/signup) and 1000 free testing requests. You can use them for tcp calls or javascript calls or both.

```csharp
ProxyCrawl.API api = new ProxyCrawl.API("YOUR_TOKEN");
```

### GET requests

Pass the url that you want to scrape plus any options from the ones available in the [API documentation](https://proxycrawl.com/dashboard/docs).

```csharp
await api.GetAsync(url, options);
```

Example:

```csharp
try {
  await api.GetAsync("https://www.facebook.com/britneyspears");
  Console.WriteLine(api.StatusCode);
  Console.WriteLine(api.OriginalStatus);
  Console.WriteLine(api.ProxyCrawlStatus);
  Console.WriteLine(api.Body);
} catch(Exception ex) {
  Console.WriteLine(ex.ToString());
}
```

You can pass any options of what the ProxyCrawl API supports in exact dictionary params format.

Example:

```csharp
await api.GetAsync("https://www.reddit.com/r/pics/comments/5bx4bx/thanks_obama/", new Dictionary<string, object>() {
  {"user_agent", "Mozilla/5.0 (Windows NT 6.2; rv:20.0) Gecko/20121202 Firefox/30.0"},
  {"format", "json"},
});

Console.WriteLine(api.StatusCode);
Console.WriteLine(api.Body);
```

### POST requests

Pass the url that you want to scrape, the data that you want to send which can be either a json or a string, plus any options from the ones available in the [API documentation](https://proxycrawl.com/dashboard/docs).

```csharp
await api.PostAsync(url, data, options);
```

Example:

```csharp
await api.PostAsync("https://producthunt.com/search", new Dictionary<string, object>() {
  {"text", "example search"},
});
Console.WriteLine(api.StatusCode);
Console.WriteLine(api.Body);
```

You can send the data as application/json instead of x-www-form-urlencoded by setting options `post_content_type` as json.

```csharp
await api.PostAsync("https://httpbin.org/post", new Dictionary<string, object>() {
  {"some_json", "with some value"},
}, new Dictionary<string, object>() {
  {"post_content_type", "json"},
});
Console.WriteLine(api.StatusCode);
Console.WriteLine(api.Body);
```

### Javascript requests

If you need to scrape any website built with Javascript like React, Angular, Vue, etc. You just need to pass your javascript token and use the same calls. Note that only `GetAsync` is available for javascript and not `PostAsync`.

```csharp
ProxyCrawl.API api = new ProxyCrawl.API("YOUR_JAVASCRIPT_TOKEN");
```

```csharp
await api.GetAsync("https://www.nfl.com");
Console.WriteLine(api.StatusCode);
Console.WriteLine(api.Body);
```

Same way you can pass javascript additional options.

```csharp
await api.GetAsync("https://www.freelancer.com", new Dictionary<string, object>() {
  {"page_wait", "5000"},
});
Console.WriteLine(api.StatusCode);
```

## Original status

You can always get the original status and proxycrawl status from the response. Read the [ProxyCrawl documentation](https://proxycrawl.com/dashboard/docs) to learn more about those status.

```csharp
await api.GetAsync("https://sfbay.craigslist.org/");
Console.WriteLine(api.OriginalStatus);
Console.WriteLine(api.ProxyCrawlStatus);
```

## Scraper API usage

Initialize the Scraper API using your normal token and call the `GetAsync` method.

```csharp
ProxyCrawl.ScraperAPI scraper_api = new ProxyCrawl.ScraperAPI("YOUR_TOKEN");
```

Pass the url that you want to scrape plus any options from the ones available in the [Scraper API documentation](https://proxycrawl.com/docs/scraper-api/parameters).

```csharp
await scraper_api.GetAsync(url, options);
```

Example:

```csharp
try {
  await scraper_api.GetAsync("https://www.amazon.com/Halo-SleepSack-Swaddle-Triangle-Neutral/dp/B01LAG1TOS");
  Console.WriteLine(scraper_api.StatusCode);
  Console.WriteLine(scraper_api.Body);
} catch(Exception ex) {
  Console.WriteLine(ex.ToString());
}
```

## Leads API usage

Initialize with your Leads API token and call the `GetAsync` method.

```csharp
ProxyCrawl.LeadsAPI leads_api = new ProxyCrawl.LeadsAPI("YOUR_TOKEN");

try {
  await leads_api.GetAsync("stripe.com");
  Console.WriteLine(leads_api.StatusCode);
  Console.WriteLine(leads_api.Body);
} catch(Exception ex) {
  Console.WriteLine(ex.ToString());
}
```

If you have questions or need help using the library, please open an issue or [contact us](https://proxycrawl.com/contact).

## Screenshots API usage

Initialize with your Screenshots API token and call the `GetAsync` method.

```csharp
ProxyCrawl.ScreenshotsAPI screenshots_api = new ProxyCrawl.ScreenshotsAPI("YOUR_TOKEN");

try {
  await screenshots_api.GetAsync("https://www.apple.com");
  Console.WriteLine(screenshots_api.StatusCode);
  Console.WriteLine(screenshots_api.ScreenshotPath);
} catch(Exception ex) {
  Console.WriteLine(ex.ToString());
}
```

or specifying a file path

```csharp
ProxyCrawl.ScreenshotsAPI screenshots_api = new ProxyCrawl.ScreenshotsAPI("YOUR_TOKEN");

try {
  await screenshots_api.GetAsync("https://www.apple.com", new Dictionary<string, object>() {
    {"save_to_path", @"C:\Users\Default\Documents\apple.jpg"},
  });
  Console.WriteLine(screenshots_api.StatusCode);
  Console.WriteLine(screenshots_api.ScreenshotPath);
} catch(Exception ex) {
  Console.WriteLine(ex.ToString());
}
```

Note that `screenshots_api.GetAsync(url, options)` method accepts an [options](https://proxycrawl.com/docs/screenshots-api/parameters)

Also note that `screenshots_api.Body` is a Base64 string representation of the binary image file.
If you want to convert the body to bytes then you have to do the following:

```csharp
byte[] bytes = Convert.FromBase64String(screenshots_api.Body);
```

If you have questions or need help using the library, please open an issue or [contact us](https://proxycrawl.com/contact).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/proxycrawl/proxycrawl-net. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The library is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the Proxycrawl project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/proxycrawl/proxycrawl-net/blob/master/CODE_OF_CONDUCT.md).

---

Copyright 2021 ProxyCrawl