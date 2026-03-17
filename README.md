# NN-Core API v1.0

A pixel-based Display Entity GUI framework for Minecraft using `TextDisplay` and `Interaction` entities. 

Built on a **True-Pixel Bottom-Center** layout engine. 1 unit of measurement equals 1 literal Minecraft pixel (1 block = 16 pixels). The coordinate `X = 0, Y = 0` represents the bottom-center of your UI canvas.

## 🚀 Getting Started

### 1. Add as a Dependency

**plugin.yml**
```yaml
depend: [NN-Core]
```

**Maven (pom.xml)**
```xml
<repository>
    <id>neyji-repo</id>
    <url>[https://neyji.github.io/NN-Core-Releases/](https://neyji.github.io/NN-Core-Releases/)</url>
</repository>

<dependency>
    <groupId>net.neyjaneyji</groupId>
    <artifactId>nn-core</artifactId>
    <version>0.1</version>
    <scope>provided</scope>
</dependency>
```

**Gradle (build.gradle)**
```gradle
repositories {
    maven { url 'https://repo.yourdomain.com/releases' }
}

dependencies {
    compileOnly 'net.neyjaneyji:nNCore:0.1'
}
```

### 2. Creating a GUI
Create a class that extends `NNCoreGUI` and draw your components inside `refreshLayout()`.

```java
import net.neyjaneyji.nNCore.api.NNCoreGUI;
import net.neyjaneyji.nNCore.api.components.*;
import org.bukkit.Color;
import org.bukkit.Location;
import java.util.UUID;

public class MyGUI extends NNCoreGUI {
    
    public MyGUI(UUID ownerId, Location origin) {
        super(ownerId, origin);
        refreshLayout(); 
    }

    @Override
    public void refreshLayout() {
        cleanup(); // Always clear old entities first!
        
        // 1. Add the base Frame (Background)
        // A 96x80 pixel canvas (6x5 blocks) placed dead-center at bottom (0, 0)
        addComponent(new FrameComponent(Color.fromARGB(240, 30, 30, 35), 96, 80), 0, 0);
        
        // 2. Add elements using True Pixel Coordinates
        // Y goes UP from 0 to 80. X goes LEFT (negative) to RIGHT (positive).
        addComponent(new ButtonComponent("Click Me", 0.5f, 0.2f, p -> {
            p.sendMessage("Action Triggered!");
        }), 0, 40); // Placed perfectly in the center of the board
    }
}
```

## 🧩 Component Guide

All components are added using `addComponent(GUIComponent, x, y)`.

### Core Layout & Primitives
* **`FrameComponent(Color, widthPixels, heightPixels)`**: The background canvas. Automatically calculates true block scale. Usually placed at `(0, 0)`.
* **`HitboxComponent(width, height, onClick)`**: An invisible `Interaction` entity that registers player gaze and clicks.
* **`TextComponent(text, alignment)`**: A basic text display. Alignment can be LEFT, RIGHT, or CENTER.
* **`CardComponent(title, body)`**: A visual container with a light background to group text.
* **`ScrollComponent(List<GUIComponent>, visibleCount)`**: Acts as a window. Pass it a massive list of components, and it generates Up/Down arrows to scroll through them, displaying only `visibleCount` at a time.

### Display & Visuals
* **`ItemComponent(ItemStack, scale)`**: Renders true 3D Minecraft items and blocks onto your 2D GUI.
* **`IconComponent(ItemStack)`**: Specialized renderer for CustomModelData flat icons.
* **`ProgressBarComponent(progress, color)`**: A visual bar representing a `0.0` to `1.0` float, tinted to any `org.bukkit.Color`.

### Inputs
* **`ButtonComponent(text, width, height, onClick)`**: A clickable button with a built-in hover effect.
* **`ToggleComponent(initialState, onToggle)`**: A classic ON/OFF switch.
* **`CheckboxComponent(initialState, onToggle)`**: A `[ ]` / `[✔]` checkbox.
* **`StepperComponent(initialValue, min, max, onChange)`**: A numeric selector with `[-]` and `[+]` buttons.
* **`SliderComponent(initialValue, onChange)`**: A visual progress bar that acts as a slider (clicks increment value).
* **`RadioButtonComponent(List<String> options, onSelect)`**: A mutually exclusive list. Clicking one deselects the others.
* **`DropdownComponent(List<String> options, defaultText, onSelect)`**: A button that expands into a vertical list of options when clicked.
* **`SearchBarComponent(placeholder, onSearch)`**: Spawns a search box that prompts the player in chat and returns the query as a `String`.

### Navigation & Feedback
* **`NavBarComponent()`**: Use `.addLink("Name", action)` to create a horizontal menu bar.
* **`BreadcrumbComponent(String... path)`**: Generates a "Home > Settings > Visuals" text trail (defaults to LEFT alignment).
* **`NotificationComponent(plugin, message, icon)`**: Spawns a toast message that automatically deletes itself after 5 seconds.
* **`ErrorMessageComponent(message)`**: A simple red text alert.
* **`ModalComponent(content)`**: Spawns a dark tint over the screen with a centered alert box.
