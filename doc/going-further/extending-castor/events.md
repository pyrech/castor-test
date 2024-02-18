# Listening to events

Castor provides utilities to listen and react when event happened inside your
project. It allows to implement custom logic at various points in the
application lifecycle.

## Registering a listener

You can register a listener inside your Castor project by using the
`Castor\Attribute\AsListener` attribute. This attribute allows you to specify
the targeted event and the priority of this listener.

```php
#[AsListener(event: AfterApplicationInitializationEvent::class)]
#[AsListener(event: AfterExecuteTaskEvent::class, priority: 1)]
function my_event_listener(AfterApplicationInitializationEvent|AfterExecuteTaskEvent $event): void
{
    // Custom logic to handle the events
}
```

> [!NOTE]
> You can specify multiple events for a single listener.

> [!NOTE]
> The `priority` argument is optional and defaults to `0`. The higher the
> priority, the earlier the listener will be executed.

## Built-in events

Here is the built-in events triggered by Castor:

* `Castor\Event\AfterApplicationInitializationEvent`: This event is triggered
  after the application has been initialized. It provides access to the
  `Application` instance and an array of `TaskDescriptor` objects;

* `Castor\Event\BeforeExecuteTaskEvent`: This event is triggered before
  executing a task. It provides access to the `TaskCommand` instance;

* `Castor\Event\AfterExecuteTaskEvent`: This event is triggered after executing
  a task. It provides access to the `TaskCommand` instance.
