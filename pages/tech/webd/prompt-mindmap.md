---
layout : page
title : Prompt Mind-map of an App
date : 2025-04-15
---

This is a novel idea by me. In the age of vibe coding, there are 2 things more important than the source code of a large app. They are 

(a) its process-flow mindmap : what are the different components in the app code and how to they interact at run-time to generate UX 

(b) the component-wise AI prompts to regenerate the entire app from scratch using an AI code assistant. 

What if both of these can be combined into a single mind-map? I call that the prompt mind-map (PMM) of the app. It will be like the blueprint or negative film of the app : like the soul of the person. Say your app's sourcecode is destroyed or a series of vibe coding has garbled and bloated up your code beyond your cognitive control, then this mindmap will be able to reproduce your entire sourcecode in seconds.

You can create this mindmap in paper. But I don't prefer it because a PMM is a living blueprint. Continuous amendments are its defining feature. As the app scales in features, the size of the mindmap may go beyond normal page size. That's why I don't prefer my favourite Goodnotes on iPad for this purpose. Instead I tried the PlantUMLQ and Mermaid code-to-diagram tools. The code is simple and nice flowcharts are generated in the editor (various free online editors and even vscode extensions are available). But in these I can't draw freestyle when I want, plus the canvas can't be moved, zoomed in or out. Therefore I tried Miro. This is great and even has an AI assistant to convert my text prompt into rough designs on the canvas in its online editor. I can store each design (pertaining to each app) as a separate file in my machine or in Google Drive. And amend it whenever I want. There is one concern though. Its designs don't have a corresponding code file. Or maybe they have but its under the hood. What if tomorrow Miro stops its free tier. I will have to buy subscription to even open, let one amending, my existing mindmap designs. This is why I settled for Excalidraw.

I had used excalidraw in the website. I can save a design as a local file or link. But I won't have the corresponding code then. So I decided to install the Excalidraw extension in VSCode/Cursor. And in the app' root folder of each app I work, I create a blank file called promptmap.excalidraw. VSC/Cursor provides an icon to switch between the code and visual views of this file. You can create any design in the visual interactive UI and corresponding code will be typed. The code is similar to json. I would suggest to first create a skeleton of the design file using the following code:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://marketplace.visualstudio.com/items?itemName=pomdtr.excalidraw-editor",
  "elements": [],
  "appState": {
    "gridSize": 20,
    "gridStep": 5,
    "gridModeEnabled": false,
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
```

After this paint the canvas !
