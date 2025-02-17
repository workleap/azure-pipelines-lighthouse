# Lighthouse for Azure Pipelines

This is an [Azure DevOps extension](https://marketplace.visualstudio.com/items?itemName=GSoft.lighthouse-vsts) that allows you to **enhance your Azure Pipelines with a Lighthouse tab**.
The Lighthouse tab embed one or many HTML reports generated by [Google Lighthouse](https://developers.google.com/web/tools/lighthouse/).

The task only requires a target URL to execute Lighthouse against.

If the [Lighthouse NPM package](https://www.npmjs.com/package/lighthouse) is already installed locally or globally on the agent, then the task will use it.
Otherwise, it will install the latest version in a temporary folder.

![Lighthouse HTML report embed in a tab](https://raw.githubusercontent.com/gsoft-inc/azure-pipelines-lighthouse/main/docs/lh-result.png)

You can also specify **audit score assertions** that can make the pipeline fail based on the audit scores.

For example, the Google Lighthouse audit `is-on-https` produces a boolean score that will be equal to `1` if the site uses HTTPS.
This is an example of a score assertion for this audit:

```
is-on-https = 1
```

Which means that you expect the audit `is-on-https` to have a score equal to `1`.

**Any audit score generated by Lighthouse is between 0 and 1**. You can also use the `>` (greater than) and the `<` (lower than) operator to evaluate decimal audit scores.
Audit score assertions must be written on separate lines.

![Lighthouse task](https://raw.githubusercontent.com/gsoft-inc/azure-pipelines-lighthouse/main/docs/lh-task.png)

## Installation

Lighthouse for Azure Pipelines can be installed from the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=GSoft.lighthouse-vsts).

## Lighthouse audits that can be used in assertions

The following audits are available for assertions in **Lighthouse 10.1.0**.
They may change with new versions.
Execute `lighthouse --list-all-audits` to discover the available audits for your desired Lighthouse version.

| Audit                        | Type    | Description                                                                                                                        |
|------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------|
| is-on-https                  | binary  | Uses HTTPS                                                                                                                         |
| service-worker               | binary  | Does not register a service worker that controls page and `start_url`                                                              |
| viewport                     | binary  | Has a `<meta name="viewport">` tag with `width` or `initial-scale`                                                                 |
| first-contentful-paint       | numeric | First Contentful Paint                                                                                                             |
| largest-contentful-paint     | numeric | Largest Contentful Paint                                                                                                           |
| first-meaningful-paint       | numeric | First Meaningful Paint                                                                                                             |
| speed-index                  | numeric | Speed Index                                                                                                                        |
| total-blocking-time          | numeric | Total Blocking Time                                                                                                                |
| max-potential-fid            | numeric | Max Potential First Input Delay                                                                                                    |
| cumulative-layout-shift      | numeric | Cumulative Layout Shift                                                                                                            |
| errors-in-console            | binary  | No browser errors logged to the console                                                                                            |
| server-response-time         | binary  | Initial server response time was short                                                                                             |
| interactive                  | numeric | Time to Interactive                                                                                                                |
| redirects                    | numeric | Avoid multiple page redirects                                                                                                      |
| installable-manifest         | binary  | Web app manifest or service worker do not meet the installability requirements                                                     |
| splash-screen                | binary  | Is not configured for a custom splash screen                                                                                       |
| themed-omnibox               | binary  | Does not set a theme color for the address bar.                                                                                    |
| maskable-icon                | binary  | Manifest doesn't have a maskable icon                                                                                              |
| content-width                | binary  | Content is sized correctly for the viewport                                                                                        |
| image-aspect-ratio           | binary  | Displays images with correct aspect ratio                                                                                          |
| image-size-responsive        | binary  | Serves images with appropriate resolution                                                                                          |
| deprecations                 | binary  | Avoids deprecated APIs                                                                                                             |
| mainthread-work-breakdown    | numeric | Minimizes main-thread work                                                                                                         |
| bootup-time                  | numeric | JavaScript execution time                                                                                                          |
| uses-rel-preconnect          | numeric | Preconnect to required origins                                                                                                     |
| font-display                 | binary  | All text remains visible during webfont loads                                                                                      |
| third-party-summary          | binary  | Minimize third-party usage                                                                                                         |
| no-unload-listeners          | binary  | Avoids `unload` event listeners                                                                                                    |
| unsized-images               | binary  | Image elements have explicit `width` and `height`                                                                                  |
| valid-source-maps            | binary  | Page has valid source maps                                                                                                         |
| aria-allowed-attr            | binary  | `[aria-*]` attributes match their roles                                                                                            |
| aria-hidden-body             | binary  | `[aria-hidden="true"]` is not present on the document `<body>`                                                                     |
| aria-hidden-focus            | binary  | `[aria-hidden="true"]` elements contain focusable descendents                                                                      |
| aria-valid-attr-value        | binary  | `[aria-*]` attributes have valid values                                                                                            |
| aria-valid-attr              | binary  | `[aria-*]` attributes are valid and not misspelled                                                                                 |
| button-name                  | binary  | Buttons have an accessible name                                                                                                    |
| color-contrast               | binary  | Background and foreground colors have a sufficient contrast ratio                                                                  |
| document-title               | binary  | Document has a `<title>` element                                                                                                   |
| duplicate-id-active          | binary  | `[id]` attributes on active, focusable elements are unique                                                                         |
| duplicate-id-aria            | binary  | ARIA IDs are unique                                                                                                                |
| heading-order                | binary  | Heading elements appear in a sequentially-descending order                                                                         |
| html-has-lang                | binary  | `<html>` element has a `[lang]` attribute                                                                                          |
| html-lang-valid              | binary  | `<html>` element has a valid value for its `[lang]` attribute                                                                      |
| image-alt                    | binary  | Image elements have `[alt]` attributes                                                                                             |
| link-name                    | binary  | Links have a discernible name                                                                                                      |
| list                         | binary  | Lists contain only `<li>` elements and script supporting elements (`<script>` and `<template>`).                                   |
| listitem                     | binary  | List items (`<li>`) are contained within `<ul>`, `<ol>` or `<menu>` parent elements                                                |
| meta-viewport                | binary  | `[user-scalable="no"]` is not used in the `<meta name="viewport">` element and the `[maximum-scale]` attribute is not less than 5. |
| uses-long-cache-ttl          | numeric | Uses efficient cache policy on static assets                                                                                       |
| total-byte-weight            | numeric | Avoids enormous network payloads                                                                                                   |
| offscreen-images             | numeric | Defer offscreen images                                                                                                             |
| render-blocking-resources    | numeric | Eliminate render-blocking resources                                                                                                |
| unminified-css               | numeric | Minify CSS                                                                                                                         |
| unminified-javascript        | numeric | Minify JavaScript                                                                                                                  |
| unused-css-rules             | numeric | Reduce unused CSS                                                                                                                  |
| unused-javascript            | numeric | Reduce unused JavaScript                                                                                                           |
| modern-image-formats         | numeric | Serve images in next-gen formats                                                                                                   |
| uses-optimized-images        | numeric | Efficiently encode images                                                                                                          |
| uses-text-compression        | numeric | Enable text compression                                                                                                            |
| uses-responsive-images       | numeric | Properly size images                                                                                                               |
| efficient-animated-content   | numeric | Use video formats for animated content                                                                                             |
| duplicated-javascript        | numeric | Remove duplicate modules in JavaScript bundles                                                                                     |
| legacy-javascript            | numeric | Avoid serving legacy JavaScript to modern browsers                                                                                 |
| doctype                      | binary  | Page has the HTML doctype                                                                                                          |
| charset                      | binary  | Properly defines charset                                                                                                           |
| dom-size                     | numeric | Avoids an excessive DOM size                                                                                                       |
| geolocation-on-start         | binary  | Avoids requesting the geolocation permission on page load                                                                          |
| inspector-issues             | binary  | No issues in the `Issues` panel in Chrome Devtools                                                                                 |
| no-document-write            | binary  | Avoids `document.write()`                                                                                                          |
| notification-on-start        | binary  | Avoids requesting the notification permission on page load                                                                         |
| paste-preventing-inputs      | binary  | Allows users to paste into input fields                                                                                            |
| uses-http2                   | numeric | Use HTTP/2                                                                                                                         |
| uses-passive-event-listeners | binary  | Uses passive listeners to improve scrolling performance                                                                            |
| meta-description             | binary  | Document has a meta description                                                                                                    |
| http-status-code             | binary  | Page has successful HTTP status code                                                                                               |
| font-size                    | binary  | Document uses legible font sizes                                                                                                   |
| link-text                    | binary  | Links have descriptive text                                                                                                        |
| crawlable-anchors            | binary  | Links are crawlable                                                                                                                |
| is-crawlable                 | binary  | Page isn’t blocked from indexing                                                                                                   |
| robots-txt                   | binary  | robots.txt is valid                                                                                                                |
| tap-targets                  | binary  | Tap targets are sized appropriately                                                                                                |
| hreflang                     | binary  | Document has a valid `hreflang`                                                                                                    |
| plugins                      | binary  | Document avoids plugins                                                                                                            |
| canonical                    | binary  | Document has a valid `rel=canonical`                                                                                               |
| bf-cache                     | binary  | Page prevented back/forward cache restoration                                                                                      |


## Build and release process

There are GitHub actions that take care of testing, packaging and publishing the extension.

## License

Copyright © 2023, Groupe GSoft Inc. This code is licensed under the Apache License, Version 2.0. You may obtain a copy of this license at https://github.com/workleap/azure-pipelines-lighthouse/blob/main/LICENSE.
