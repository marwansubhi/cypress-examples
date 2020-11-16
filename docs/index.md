---
sidebar: false
---

# Cypress Examples

> Static site with Cypress examples tested right from the Markdown sources

## Commands

Commands drive your tests in the browser like a real user would. They let you perform actions like typing, clicking, xhr requests, and can also assert things like "my button should be disabled".

- [Querying](./commands/querying.md)
  - [get](./commands/querying.md#cy-get)
  - [contains](./commands/querying.md#cy-contains)
  - [within](./commands/querying.md#within)
  - [root](./commands/querying.md#root)
  - [Best Practices: Selecting elements](./commands/querying.md#best-practices-selecting-elements)
- [Traversal](./commands/traversal.md)
  - [children](./commands/traversal.md#children)
  - [closest](./commands/traversal.md#closest)
  - [eq](./commands/traversal.md#eq)
  - [filter](./commands/traversal.md#filter)
  - [find](./commands/traversal.md#find)
  - [first](./commands/traversal.md#first)
  - [last](./commands/traversal.md#last)
  - [next](./commands/traversal.md#next)
  - [nextAll](./commands/traversal.md#nextAll)
  - [nextUntil](./commands/traversal.md#nextUntil)
  - [not](./commands/traversal.md#not)
  - [parent](./commands/traversal.md#parent)
  - [parents](./commands/traversal.md#parents)
  - [parentsUntil](./commands/traversal.md#parentsUntil)
  - [prev](./commands/traversal.md#prev)
  - [prevAll](./commands/traversal.md#prevAll)
  - [prevUntil](./commands/traversal.md#prevUntil)
  - [siblings](./commands/traversal.md#siblings)
- [Actions](./commands/actions.md)
  - [type](./commands/actions.md#type)
  - [focus](./commands/actions.md#focus)
- [Viewport](./commands/viewport.md)
  - [viewport](./commands/viewport.md#cy-viewport)
- [Location](./commands/location.md)
  - [hash](./commands/location.md#cy-hash)
  - [location](./commands/location.md#cy-location)
  - [url](./commands/location.md#cy-url)
- [Window](./commands/window.md)
  - [window](./commands/window.md#cy-window)
  - [document](./commands/window.md#cy-document)
  - [title](./commands/window.md#cy-title)
- [Assertions](./commands/assertions.md)
  - [Implicit Assertions](./commands/assertions.md#implicit-assertions)
    - [should](./commands/assertions.md#should)
    - [and](./commands/assertions.md#and)
  - [Explicit Assertions](./commands/assertions.md#explicit-assertions)
    - [expect](./commands/assertions.md#expect)
    - [assert](./commands/assertions.md#assert)
  - [Should with callback function](./commands/assertions.md#should-with-callback-function)
- [Navigation](./commands/navigation.md)
  - [go](./commands/navigation.md#cy-go)
  - [reload](./commands/navigation.md#cy-reload)
  - [visit](./commands/navigation.md#cy-visit)
- [Aliasing](./commands/aliasing.md)
  - [as](./commands/aliasing.md#as)
- [Waiting](./commands/waiting.md)
  - [wait](./commands/waiting.md#cy-wait)
- [Misc](./commands/misc.md)
  - [end](./commands/misc.md#end)
  - [screenshot](./commands/misc.md#cy-screenshot)
  - [wrap](./commands/misc.md#cy-wrap)
- [Connectors](./commands/connectors.md)
  - [then](./commands/connectors.md#then)
- [Network Requests](./commands/network-requests.md)
  - [server](./commands/network-requests.md#cy-server)
  - [request](./commands/network-requests.md#cy-request)
  - [route](./commands/network-requests.md#cy-route)
- [Local Storage](./commands/local-storage.md)
  - [clearLocalStorage](./commands/local-storage.md#cy-clearlocalstorage)
- [Files](./commands/files.md)
  - [fixture](./commands/files.md#cy-fixture)
  - [fixture or require](./commands/files.md#cy-fixture-or-require)
  - [readFile](./commands/files.md#cy-readfile)
  - [writeFile](./commands/files.md#cy-writefile)
- [Cookies](./commands/cookies.md)
  - [getCookie](./commands/cookies.md#cy-getcookie)
  - [getCookies](./commands/cookies.md#cy-getcookies)
  - [setCookie](./commands/cookies.md#cy-setcookie)
  - [clearCookie](./commands/cookies.md#cy-clearcookie)
  - [clearCookies](./commands/cookies.md#cy-clearcookies)
- [Spies, Stubs & Clocks](./commands/spies-stubs-clocks.md)
  - [spy](./commands/spies-stubs-clocks.md#cy-spy)
  - [stub](./commands/spies-stubs-clocks.md#cy-stub)
  - [clock](./commands/spies-stubs-clocks.md#cy-clock)
  - [tick](./commands/spies-stubs-clocks.md#cy-tick)

## Utilities

Utilities give you access to methods from other commonly used libraries.

- [\_](./utilities/index.md#cypress)
- [\$](./utilities/index.md#cypress-2)
- [Blob](./utilities/index.md#cypress-blob)
- [minimatch](./utilities/index.md#cypress-minimatch)
- [moment](./utilities/index.md#cypress-moment)
- [Promise](./utilities/index.md#cypress-promise)

## Cypress API

The Cypress API enables you to configure the behavior of how Cypress works internally. You can do things like access Environment Variables, change configuration, create custom commands, and more.

- [Commands](./cypress-api/index.md#cypress-commands-add)
- [Cookies](./cypress-api/index.md#cypress-cookies-debug)
- [Server](./cypress-api/index.md#cypress-server-default)
- [arch](./cypress-api/index.md#cypress-arch)