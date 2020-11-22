# Build an Emergency Response solution with Teams and SharePoint

[![Video thumbnail](./Labs/images/videoThumbnail.png)](https://youtu.be/JaQSJsxOS0E)
*Watch the presentation here*

In this half day workshop, you will learn how to build solutions with Microsoft Teams and Microsoft SharePoint. In the process, you will build a simple Emergency Response Center which could be used to coordinate the response to an emergency such as a natural disaster. This could complement the [Crisis Communication Power App template](https://powerapps.microsoft.com/en-us/blog/crisis-communication-a-power-platform-template/), which helps to keep employees informed during a crisis; this solution is for the reponse team themselves.

![Completed solution](./Labs/images/Demo-Mashup.png)

You will build:

 * A SharePoint communication site for publishing news to the response team and others
 * A tab for coordinating supplies and where they're needed, based on a SharePoint list
 * A tab for viewing problem locations on a map, written in SharePoint Framework and backed by another SharePoint list using Microsoft Graph

This repository contains the entire workshop, including [presentation material](Presentation.md), source code, and step-by-step instructions.

To complete the hands-on portion of the lab, you will need:

 * A computer with a web browser and an Internet connection (a phone is too small to successfully construct the solution, though it is usable from a mobile device.)
 * A Microsoft 365 Developer subscription (get one free at [https://aka.ms/M365DeveloperProgram](https://developer.microsoft.com/microsoft-365/dev-program?WT.mc_id=M365-github-rogerman), or one will be provided for you by the workshop facilitator.)
 * A Bing Maps API key
 * For the coding exercises, you will need to install development tools on your computer:
    * [Node Version Manager (nvm)](http://npm.github.io/installation-setup-docs/installing/using-a-node-version-manager.html) (optional but recommended to allow switching Node versions)
    * [Node.js](https://nodejs.org/en/) (SharePoint Framework currently requires version LTS 10.x)
    * A code editor such as [Visual Studio Code](https://code.visualstudio.com/download?WT.mc_id=M365-github-rogerman)

    Completed builds are included here as well, so if you don't need to install anything on your computer if you'd prefer to just follow along and use the pre-built packages.

## The Workshop

 * [Presentation](Presentation.md)
 * [Exercise 1: Lab setup](Labs/Part1.md)
 * [Exercise 2: SharePoint News](Labs/Part2.md)
 * [Exercise 3: SharePoint List Tab](Labs/Part3.md)
 * [Exercise 4: SharePoint Framework tabs](Labs/Part4.md)
 * [Exercise 5: Calling the Microsoft Graph](Labs/Part5.md)
 * [Resources](Labs/Resources.md)
 
## Using the labs

There are a lot of screen shots, which always appear _after_ the written instruction they refer to. Call-outs in the screen shots are referenced in the text using keycaps ( 1️⃣, 2️⃣, etc.)

If you're viewing these instructions in a web browser, note that none of the links open in a new tab, so if you want to view some source code you may want to right click and open the new tab so you don't lose your place.

If you're viewing these instructions in Teams, links to source code will actually download the file in question. Consider downloading the whole repo and editing it locally.

The documentation includes some asides that can safely be skipped when following a list of instructions:

---
😎 BEST PRACTICE: Really there are no "best practices", only tradeoffs. In a lab or hack, the tradeoffs are made in favor of simplicity; these notations recommend approaches to consider working on a real project.

---

---
⛏️ DIG DEEPER: Check out these entries if you're interested in more technical detail.

---

---
🏁 CHALLENGE: Ideas for going beyond the lab exercises to learn more on your own.

---

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
