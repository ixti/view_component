---
layout: default
title: Instrumentation
parent: How-to guide
---

# Instrumentation

Since 2.34.0
{: .label }

To enable ActiveSupport notifications, use the `instrumentation_enabled` option:

```ruby
# config/application.rb
# Enable ActiveSupport notifications for all ViewComponents
config.view_component.instrumentation_enabled = true
config.view_component.use_deprecated_instrumentation_name = false
```

Setting `use_deprecated_instrumentation_name` configures the event name. If `false` the name is `"render.view_component"`. If `true` (default) the deprecated `"!render.view_component"` will be used.

Subscribe to the event:

```ruby
ActiveSupport::Notifications.subscribe("render.view_component") do |*args| # or !render.view_component
  event = ActiveSupport::Notifications::Event.new(*args)
  event.name    # => "render.view_component"
  event.payload # => { name: "MyComponent", identifier: "/Users/mona/project/app/components/my_component.rb" }
end
```

## Viewing instrumentation sums in the browser developer tools

When using `render.view_component` with `config.server_timing = true` (default in development) in Rails 7, the browser developer tools display the sum total timing information in Network > Timing under the key `render.view_component`.

![Browser showing the Server Timing data in the browser dev tools](../images/viewing_instrumentation_sums_in_browser_dev_tools.png "Server Timing data in the browser dev tools")
