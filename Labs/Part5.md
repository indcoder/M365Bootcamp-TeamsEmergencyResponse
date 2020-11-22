# Exercise 5: Calling the Microsoft Graph

 * [Presentation](../Presentation.md)
 * [Exercise 1: Lab setup](Part1.md)
 * [Exercise 2: SharePoint News](Part2.md)
 * [Exercise 3: SharePoint List Tab](Part3.md)
 * [Exercise 4: SharePoint Framework tabs](Part4.md)
 * [Exercise 5: Calling the Microsoft Graph](Part5.md) **(You are here)**
 * [Resources](Resources.md)

In this exercise, you'll add code to the solution which posts a message in the Teams channel when a new map point is added. The completed solution is in the ["Exercise 5" branch](https://github.com/OfficeDev/M365Bootcamp-TeamsEmergencyResponse/tree/Exercise5/Solution/MapViewer) of this repo.

If you don't have developer tools, grab the `map-view.sppkg` file from the Exercise 5 folder and skip to Step 3.

## Step 1: Modify the code

a. Ensure you have downloaded or cloned this repository, and open the [Solution/MapViewer](../Solution/MapViewer/) folder in your code editor.

The [src/webparts/mapViewer/services/GraphService](../Solution/MapViewer/src/webparts/mapViewer/services/GraphService) folder contains the Microsoft Graph code for the solution. Open `IGraphService.ts` and add a function to the `IGraphService` interface:

~~~typescript
sendToChannel(message: string): Promise<void | string>;
~~~

The completed file is [here](https://github.com/OfficeDev/M365Bootcamp-TeamsEmergencyResponse/blob/Exercise5/Solution/MapViewer/src/webparts/mapViewer/services/GraphService/IGraphService.ts).

b. In the `GraphService.ts` file, import a reference to the Teams JavaScript SDK at the top of the file.

~~~typescript
import * as microsoftTeams from "@microsoft/teams-js";
~~~

Then add a function to get the Teams context from the Teams JavaScript SDK.

~~~typescript
private async getTeamsContext(): Promise<any> {

    return new Promise<any> ((resolve, reject) => {
        if (microsoftTeams) {
            microsoftTeams.getContext((context) => {
                resolve(context);
            });
        } else {
            reject ("Error: Teams context not found");
        }    
    });
}
~~~

Finally, add a function to send a message to the current Teams channel using the Microsoft Graph.

~~~typsecript
public async sendToChannel(message: string): Promise<void | string> {

    const teamsContext = await this.getTeamsContext();
    const teamId = teamsContext.groupId;
    const channelId = teamsContext.channelId;

    return new Promise<void | string>((resolve, reject) => {
        this.serviceProps.graphClient.api(
            `/teams/${teamId}/channels/${channelId}/messages`)
            .post({
                body: {
                    content: message,
                    contentType: "text"
                }
            }, ((err, res) => {
                if (!err) {
                    resolve();
                } else {
                    reject(err);
                }
            }));
    });
}
~~~

The completed file is [here](https://github.com/OfficeDev/M365Bootcamp-TeamsEmergencyResponse/blob/Exercise5/Solution/MapViewer/src/webparts/mapViewer/services/GraphService/GraphService.ts).

c. The [src/webparts/mapViewer/services/MapDataService](../Solution/MapViewer/src/webparts/mapViewer/services/MapDataService) folder contains the code that reads and updates map points using the Graph service and the Bing Maps service. Modify [MapDataService.ts](../Solution/MapViewer/src/webparts/mapViewer/services/MapDataService/MapDataService.ts) to add a call to the new `sendToChannel()` function. Add it to the `getMapPoints()` function 
where you see the comment,

~~~typescript
/* 
 * Add the call to sendToChannel() here 
 */
~~~

Here is the snippet of code to add:

~~~typescript
await this.serviceProps.graphService.sendToChannel(
    `New point ${p.title} added at ${p.latitude}, ${p.longitude}`
);
~~~

The updated `getMapPoints()` function should look like this:

~~~typescript
public async getMapPoints(geocode: boolean): Promise<ILocation[]> {

    const locationMapper = new LocationMapper();

    let listId: string;
    try {
        listId = await this.serviceProps.graphService.getListId(
            this.serviceProps.siteId, this.serviceProps.listName
        );    
    }
    catch (error) {
        if (error.statusCode === 404) {
            listId = await this.serviceProps.graphService.createList(
                this.serviceProps.siteId, this.serviceProps.listName,
                locationMapper
            );
        } else throw(error);
    }

    const points = await this.serviceProps.graphService.getListItems<ILocation>(
        this.serviceProps.siteId, listId, locationMapper
    );

    if (geocode) {
        for (let p of points) {
            if ((!p.latitude || !p.longitude) &&
                    (p.address || p.city || p.stateProvince || p.country)) {
                
                // If here, we're missing the geo-coordinates for an item and have
                // address or other info. Try to geocode it.
                let coordinates = await this.serviceProps.bingMapsService.geoCode(
                    p.country, p.stateProvince, p.city, p.address
                );

                if (typeof coordinates === 'object') {
                    // If here, the geocode was succesful - update the item
                    p.latitude = coordinates.latitude;
                    p.longitude = coordinates.longitude;
                    await this.serviceProps.graphService.updateListItem(
                        this.serviceProps.siteId, listId, locationMapper, p.id,
                        {
                            latitude: p.latitude,
                            longitude: p.longitude
                        }
                    );
                    await this.serviceProps.graphService.sendToChannel(
                        `New point ${p.title} added at ${p.latitude}, ${p.longitude}`
                    );
                }
            }
        }
    }

    return (points);
}
~~~

The completed file is [here](https://github.com/OfficeDev/M365Bootcamp-TeamsEmergencyResponse/blob/Exercise5/Solution/MapViewer/src/webparts/mapViewer/services/MapDataService/MapDataService.ts).

d. We're going to need permission to use the Graph API to post in the Teams channel. To request this, edit the [config/package-solution.json](../Solution/MapViewer/config/package-solution.json) and add a new item in the `webApiPermissionRequests` array:

~~~json
      {
        "resource": "Microsoft Graph",
        "scope": "ChannelMessage.Send"
      }
~~~

The completed file is [here](https://github.com/OfficeDev/M365Bootcamp-TeamsEmergencyResponse/blob/Exercise5/Solution/MapViewer/config/package-solution.json).

## Step 2: Rebuild the SharePoint solution package

This assumes you already built the package as explained in Exercise 4 Step 1.

a. If you're running locally, you can make simple code changes without re-deploying the SharePoint solution package, but in this case we added a permission request so it's time to rebuild the package.

Return to the command line to rebuild the solution package. If you're running locally,

~~~bash
gulp bundle
gulp package-solution
~~~

If you want to deploy the JavaScript bundle to the SharePoint CDN,

~~~bash
gulp bundle --ship
gulp package-solution --ship
~~~

## Step 3: Re-deploy the SharePoint solution package and approve permissions

You need to repeat steps 2 and 3 of Exercise 4 to update your work in SharePoint.

a. Return to the SharePoint App catalog and upload the `map-viewer.sppkg` file again.

![Part4](images/Part4-03.png)

b. Return to the **API Access** screen; you should see the new permission . Select it 1️⃣ and use **Approve** 2️⃣. 

![Part4](images/Part5-01.png)

## Step 4: Test the change

a. If you're running locally (you chose Option 1 in Exercise 4), ensure your local web server is running in the terminal window. If not, start it again.

~~~bash
gulp serve --nobrowser
~~~

a. Return to Microsoft Teams and refresh the **Map View** tab. Add a new point to the map; a notification should appear in the channel.

![Part4](images/Part5-03.png)

Congratulations, you've completed all 5 parts of the workshop! Please check out [these resources](./Resources.md) and thanks for your interest in Microsoft 365 development!
