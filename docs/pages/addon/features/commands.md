# Create Commands

With LabyMod 4, you can easily create custom client-side commands. This page will show you how.

### The Super Method
The first parameter in the super constructor call is the prefix, see it as the name of your command. Every parameter after is an alias and thus optional.

### The Execute Method
The execute method contains the code that is executed if the player is using your command. If you return `true`, the command will be consumed by LabyMod. If you return `false`, the command will not be consumed and will be sent to the server.

Before you're able to use and test your command in-game, you'll need to register the command by calling  `this.registerCommand(new ExampleCommand());` in your main class. You don't need to do anything else, after registering the command and restarting LabyMod you can submit "/notify" or "/alias" in the ingame-chat and your command will be executed.

=== ":octicons-file-code-16: ExampleCommand"
```java
public class ExampleCommand extends Command {

    private final NotificationController notificationController;

    public ExampleCommand() {
        super("notify", "alias");
        this.notificationController = Laby.references().notificationController();
    }

    @Override
    public boolean execute(String prefix, String[] arguments) {
        if (prefix.equalsIgnoreCase("alias")) {
            this.displayMessage(Component.text("You used an Alias!", NamedTextColor.AQUA));
            return false;
        }

        String title;
        String text;

        if (arguments.length < 2) {
            title = "Title";
            text = "Text";
        } else {
            title = arguments[0];
            text = arguments[1];
        }
        notificationController.push(Notification.builder()
                .title(Component.text(title))
                .text(Component.text(text))
                .build());
        return true;
    }
}
```