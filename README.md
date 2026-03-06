# Rive Framer Player

A Framer component to embed Rive animations with full scripting support, feathering, and all fit/alignment options.

## Features

- Rive Scripting support
- Feathering & blur effects (WebGL2)
- All fit modes (contain, cover, fill, fitWidth, fitHeight, none, scaleDown)
- All alignment options
- Custom artboard & state machine selection
- Pointer interactions (click, hover)

## Quick Start

### 1. Add the Component to Framer

In your Framer project, create a new **Code Component** and paste this code:

```tsx
import { addPropertyControls, ControlType } from "framer"

export default function RivePlayer(props) {
    const { src, artboard, stateMachine, fit, alignment, debug } = props
    
    const playerUrl = "https://tomaccoux.github.io/rive-framer-player/player.html"
    const defaultBaseUrl = "https://raw.githubusercontent.com/tomaccoux/rive-framer-player/main/"
    
    let riveSrc = src
    if (src && !src.startsWith("http")) {
        riveSrc = defaultBaseUrl + src
    }
    
    const params = new URLSearchParams()
    params.set("src", riveSrc)
    if (artboard) params.set("artboard", artboard)
    if (stateMachine) params.set("sm", stateMachine)
    params.set("fit", fit || "contain")
    params.set("align", alignment || "center")
    
    const fullUrl = `${playerUrl}?${params.toString()}`

    if (debug) {
        return (
            <div style={{ padding: 10, fontSize: 12, wordBreak: "break-all" }}>
                <strong>URL:</strong> {fullUrl}
            </div>
        )
    }

    return (
        <iframe
            src={fullUrl}
            style={{
                width: "100%",
                height: "100%",
                border: "none",
                display: "block",
            }}
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope"
        />
    )
}

addPropertyControls(RivePlayer, {
    debug: {
        type: ControlType.Boolean,
        title: "Debug Mode",
        defaultValue: false,
    },
    src: {
        type: ControlType.String,
        title: "Rive File",
        defaultValue: "lightning_test.riv",
        placeholder: "filename.riv or full URL",
    },
    artboard: {
        type: ControlType.String,
        title: "Artboard",
        defaultValue: "",
    },
    stateMachine: {
        type: ControlType.String,
        title: "State Machine",
        defaultValue: "State Machine 1",
    },
    fit: {
        type: ControlType.Enum,
        title: "Fit",
        options: ["contain", "cover", "fill", "fitWidth", "fitHeight", "none", "scaleDown"],
        optionTitles: ["Contain", "Cover", "Fill", "Fit Width", "Fit Height", "None", "Scale Down"],
        defaultValue: "contain",
    },
    alignment: {
        type: ControlType.Enum,
        title: "Alignment",
        options: ["center", "topLeft", "topCenter", "topRight", "centerLeft", "centerRight", "bottomLeft", "bottomCenter", "bottomRight"],
        optionTitles: ["Center", "Top Left", "Top Center", "Top Right", "Center Left", "Center Right", "Bottom Left", "Bottom Center", "Bottom Right"],
        defaultValue: "center",
    },
})
```

### 2. Use with Any Rive File

**Option A: Use a URL**
```
https://your-site.com/animation.riv
```

**Option B: Host on GitHub**
1. Fork this repo
2. Add your `.riv` files
3. Use just the filename: `my-animation.riv`

## URL Parameters

| Parameter | Example | Description |
|-----------|---------|-------------|
| `src` | `https://...file.riv` | Rive file URL (required) |
| `fit` | `cover` | Fit mode |
| `align` | `topLeft` | Alignment |
| `artboard` | `MyArtboard` | Artboard name |
| `sm` | `State Machine 1` | State machine name |

## Direct URL Usage

You can also use the player directly without Framer:

```
https://tomaccoux.github.io/rive-framer-player/player.html?src=YOUR_RIVE_URL&fit=cover
```

## Credits

Created by [Tom Acco](https://rivemasterclass.com), instructor of Rive Masterclass.

## License

MIT
