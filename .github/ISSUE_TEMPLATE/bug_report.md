---
name: Bug report
about: Create a report to help us improve
title: ''
labels: ''
assignees: ''

---

On this page
•	Getting started
•	Using the Isochrone API
•	Create a map
•	Add the sidebar
•	Add the Isochrone API
•	Draw the isochrone contour
•	Make the app interactive
•	Add a marker
•	Final product
•	Next steps

This tutorial provides a step-by-step guide on harnessing the Mapbox Isochrone API, leveraging Mapbox routing profiles and travel times. By following this tutorial, you will learn how to develop a web application that empowers users to gauge their potential travel distances by foot, bicycle, or car based on predefined time limits.

1.	Access Token:
•	An access token is a key that associates your usage of Mapbox services with your Mapbox account.
•	You can obtain this token from your Mapbox account. It serves as a form of authentication to access Mapbox services.
•	You can usually find your access token on your Account page after logging into your Mapbox account. If you don't have an account, you'll need to sign up for one.
•	https://www.mapbox.com/account/

2. Mapbox GL JS:
•	Mapbox GL JS serves as a fundamental component for building web maps in this tutorial. It is a JavaScript API provided by Mapbox designed explicitly for constructing interactive maps for web applications. You'll be using it to incorporate the map into your app, enabling users to visualize and interact with the isochrone data. Mapbox GL JS
3. Mapbox Isochrone API:
•	The Mapbox Isochrone API is the core of this tutorial. This API is the engine behind the calculations for regions that can be reached within specific travel times from a given location. It computes these areas and returns them in the form of contours, represented as polygons or lines. These contours can be seamlessly integrated into your map, visually showcasing areas that users can reach within defined time constraints. Isochrone API
4. Mapbox Assembly:
•	For styling the user interface (UI) of your web application, we recommend using Mapbox Assembly. Assembly is an open-source CSS framework. By applying Assembly's styles, you can create an aesthetically pleasing and user-friendly UI for your travel-time estimation app. This helps in providing an intuitive user experience. Assembly 
5. Text Editor:
•	In order to code and develop the app, you'll need a text editor. The choice of text editor is yours; you can use your preferred text editor to write HTML, CSS, and JavaScript. This is where you'll create and structure the code for your web application. It's crucial to have a text editor to craft and edit the necessary files that power your app's functionality. Downloads | Notepad++ (notepad-plus-plus.org)

Using the Isochrone API: A Comprehensive Guide
To make your travel-time estimation app work seamlessly, you need to understand how to harness the power of the Isochrone API. This API is your tool for calculating and displaying areas reachable within specific time constraints. Here's a step-by-step explanation of how to make it work:
Three Essential Parameters:
1.	Profile: This parameter determines the Mapbox routing profile your query should use. The profile depends on the type of travel you want to analyze. For example, choose "walking" for pedestrian travel times, "cycling" for bicycle travel times, or "driving" for car travel times.
2.	Coordinates: You'll need to provide a set of coordinates, which is essentially a {longitude, latitude} pair. These coordinates act as the center point around which the isochrone lines will be drawn. This point is where your travel-time calculations originate.
3.	Contours Minutes: This parameter is vital. It defines the duration of trips in minutes. You can specify one or more times, separated by commas, to indicate different time limits. Be aware that the maximum duration for each isochrone you can request is 60 minutes.
Optional Parameters:
The Isochrone API offers some optional parameters that you can use to tailor your query. For your app's specific purpose, you'll focus on one optional parameter:
•	Polygons: This parameter gives you control over whether the isochrone contours should be returned as GeoJSON polygons or linestrings. If you set "polygons" to "true," any contour that forms a ring will be returned as a polygon. This makes it easier to visualize the areas users can reach.
Constructing an API Request:
To put all these parameters together into an API request, you need to structure a URL as follows:
rubyCopy code
https://api.mapbox.com/isochrone/v1/mapbox/{profile}/{coordinates}.json?{contours_minutes}&access_token=YOUR_ACCESS_TOKEN 
Replace the placeholders like {profile}, {coordinates}, {contours_minutes}, and YOUR_ACCESS_TOKEN with your specific values. For example:
rubyCopy code
https://api.mapbox.com/isochrone/v1/mapbox/driving/-117.17282,32.71204?contours_minutes=15&polygons=true&access_token=YOUR_ACCESS_TOKEN 
In this example, the request uses the driving profile, specifies coordinates, sets a travel time of 15 minutes, and requests results in polygon format.
Inspecting the JSON Response:
After making your API request, you can paste it into your web browser's address bar to receive a JSON response. This response holds critical information for your app's functionality:
•	Geometry Object: This part of the response includes an array of coordinates that define the outlines of the isochrone contour. These coordinates are the building blocks that create the shape of the reachable area on your map.
•	Features Object: In this section, you'll find details on how the isochrone should be visually represented. It includes information about the fill color and fill opacity. This data is your toolkit for styling and drawing the isochrone contours on your app's map.


	
Creating a Map:
To get your app rolling, the first step is to build a map using Mapbox GL JS. Here's a detailed breakdown of how you can set up your HTML file for this purpose:
1. Open Your Text Editor:
Before we dive in, make sure you have a preferred text editor ready to go. You'll be writing HTML, CSS, and JavaScript, so a good text editor is your essential tool.
2. Create a New HTML File:
In your text editor, create a new file and save it with the name "index.html." This will be the main HTML file for your app.
3. HTML Setup:
Now, it's time to set up your HTML file. Copy and paste the following HTML code into your "index.html" file. This code will be the foundation for your app:



In this section, we'll break down the HTML code you've just inserted into your "index.html" file to create the foundation for your map-based app. This code is crucial as it forms the structure of your page and imports essential resources.
1. Document Structure:
htmlCopy code
<!DOCTYPE html> <html> 
•	The <!DOCTYPE html> declaration specifies the document type and version as HTML5. It's the starting point for an HTML document.
•	<html> encloses the entire HTML content. It's the root element of your HTML document.
2. Head Section:
<head> <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' /> <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v2.6.1/mapbox-gl.js'></script> <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v2.6.1/mapbox-gl.css' rel='stylesheet' /> <style> body { margin: 0; padding: 0; } #map { position: absolute; top: 0; bottom: 0; width: 100%; } </style> </head> 
•	<head> contains meta-information about the document, external resource links, and styling information.
•	<meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' /> sets the viewport properties to control the initial scale, maximum scale, and user scalability.
•	<script> and <link> tags import the Mapbox GL JS resources. The JavaScript file (mapbox-gl.js) and CSS file (mapbox-gl.css) are essential for Mapbox functionality and map style.
•	The <style> section includes CSS rules that style the webpage. In this case, it sets the body's margin and padding to zero, ensuring the map spans the entire screen. The #map selector defines the styling for the map container, making it absolute-positioned, covering the full vertical height, and having 100% width.
3. Body Section:
html
<body> <div id='map'></div> <script> mapboxgl.accessToken = 'YOUR_ACCESS_TOKEN'; var map = new mapboxgl.Map({ container: 'map', style: 'mapbox://styles/mapbox/streets-v11', center: [-74.50, 40], zoom: 9 }); </script> </body> 
•	<body> contains the visible content of the web page.
•	<div id='map'></div> creates a div element with the ID "map." This is the container where your map will be displayed on the page.
•	The <script> tag includes JavaScript code. First, it sets your Mapbox access token by assigning it to mapboxgl.accessToken. Replace 'YOUR_ACCESS_TOKEN' with your actual access token.
•	Inside the JavaScript code block, mapboxgl.Map creates your map. It specifies that the map should be displayed in the 'map' container, uses the 'mapbox://styles/mapbox/streets-v11' style, centers the map at coordinates [-74.50, 40], and sets an initial zoom level of 9.
4. Viewing the Rendered Map: Save your changes in the text editor, and then open the HTML file in your web browser. You will see a rendered map, centered on the Mapbox headquarters in Washington D.C.
This HTML structure and setup are the foundation for building your map-based web app using Mapbox GL JS. It's the starting point for adding more functionality and customizing your map to suit your needs.

 

This Code Creates an Interactive Sidebar - Detailed Explanation
The code provided creates an interactive sidebar that allows users to select their preferred mode of transportation (walking, cycling, or driving) and the amount of time they want to spend (10, 20, or 30 minutes). Here's a breakdown of the steps:
1. HTML Structure: Inside your HTML file, a <div> element is used to create a sidebar with toggle buttons.
htmlCopy code
<div class="absolute fl my24 mx24 py24 px24 bg-gray-faint round"> <h2>Select Your Options</h2> <form> <!-- Options for selecting transportation profile --> <label for="transportation">Select Transportation:</label> <select id="transportation" name="transportation"> <option value="driving">Driving</option> <option value="cycling">Cycling</option> <option value="walking">Walking</option> </select> <!-- Options for selecting the duration (in minutes) --> <label for="duration">Select Duration (minutes):</label> <input type="number" id="duration" name="duration" min="1" max="60"> <!-- Button to trigger isochrone calculation --> <button type="button" id="calculate">Calculate Isochrone</button> </form> </div> 
2. Assembly CSS Classes: The Assembly CSS framework is used to style the sidebar and its elements. Here's how the applied classes affect the sidebar's appearance:
•	absolute: Positions the sidebar absolutely within its parent container.
•	fl: Floats the sidebar to the left within the parent container.
•	my24: Adds a margin of 24 pixels on the vertical axis (top and bottom).
•	mx24: Adds a margin of 24 pixels on the horizontal axis (left and right).
•	py24: Adds padding of 24 pixels on the vertical axis (top and bottom).
•	px24: Adds padding of 24 pixels on the horizontal axis (left and right).
•	bg-gray-faint: Sets the background color of the sidebar to a light gray shade.
•	round: Adds a border radius to round the corners of the sidebar.
3. Sidebar Content: Within the sidebar, the following elements are included:
•	<h2>Select Your Options</h2>: A heading that provides context for the user.
•	A <form> element containing the following interactive components:
•	A dropdown menu (<select>) for selecting the desired mode of transportation (walking, cycling, or driving).
•	An input field (<input>) for entering the duration (in minutes) that the user wants to spend. The min and max attributes ensure the input is limited to values between 1 and 60.
•	A button (<button>) with the text "Calculate Isochrone" that users will click to initiate the isochrone calculation.
4. Styling Considerations: The use of Assembly classes streamlines the styling process. However, if you prefer, you can apply custom CSS or use an alternative CSS framework to achieve the desired visual appearance.
5. Interactive Functionality: Although the sidebar looks good, at this point, clicking the buttons does not trigger any actions. In the next step, you will add a call to the Isochrone API, allowing you to connect the user interface to create an interactive app. This means that when users make selections in the sidebar, the app will respond by calculating and displaying isochrone maps based on their choices.
6. Save and Preview: Be sure to save your work, and when you refresh the web page, the sidebar will display on the left side. Users will be able to interact with the sidebar to customize their isochrone queries.
You are now prepared to make your web app interactive by linking these UI elements to the Isochrone API.
 




Integrating the Isochrone API - Step by Step Explanation
To integrate the Isochrone API into your web app and enable it to calculate isochrone maps based on user selections, you'll create a JavaScript function named getIso. This function will construct the API request URL and use the fetch method to make a call to the Isochrone API. Here's a detailed breakdown of this process:
1. Create the getIso Function:
javascriptCopy code
// Define the getIso function function getIso() { // Construct the Isochrone API URL const profile = document.getElementById("transportation").value; // Get the selected transportation profile (driving, cycling, walking). const duration = document.getElementById("duration").value; // Get the selected duration (in minutes). const coordinates = map.getCenter().wrap(); // Get the map's current center coordinates. // Build the Isochrone API request URL const apiUrl = `https://api.mapbox.com/isochrone/v1/mapbox/${profile}/${coordinates.lng},${coordinates.lat}?contours_minutes=${duration}&polygons=true&access_token=${mapboxgl.accessToken}`; // Make the API request using the fetch method fetch(apiUrl) .then((response) => response.json()) // Convert the response to JSON format .then((data) => { // Process the JSON response data (You will add code here to handle the response) }) .catch((error) => { // Handle any errors that occur during the fetch console.error('Error:', error); }); } 
2. Function Explanation:
•	getIso Function: This function is defined to handle the Isochrone API request. It extracts user selections (transportation profile and duration) and the map's current center coordinates.
•	Request Parameters:
•	profile: It retrieves the selected transportation profile (driving, cycling, walking) from the HTML dropdown menu.
•	duration: It gets the selected duration (in minutes) entered by the user.
•	coordinates: It fetches the map's current center coordinates (longitude and latitude).
•	API Request URL: The apiUrl variable is constructed by combining these parameters into a valid Isochrone API request URL. It includes:
•	The selected transportation profile (profile)
•	The map's center coordinates (coordinates)
•	The selected duration (duration)
•	The polygons=true parameter to specify the response format as GeoJSON polygons
•	Your Mapbox access token for authentication
•	Making the API Request: The fetch method is used to send a GET request to the constructed API URL.
•	Handling the Response: You will add code within the .then() block to process the JSON response data from the Isochrone API. This code will include actions to draw and style the isochrone contours on the map based on the response.
•	Error Handling: The .catch() block is included to handle and log any errors that may occur during the fetch operation.
3. Connecting UI to the getIso Function: To make this function execute when users click the "Calculate Isochrone" button, you will link this function to the button click event. This will be demonstrated in the following steps of your tutorial.


ackground:
In the previous step, you successfully sent a request to the Isochrone API and printed the response to the console. Now, you're moving on to the crucial task of displaying this isochrone contour on the map. To do this, you will set up a source and a layer in Mapbox GL JS, which is a common approach to add custom data to a Mapbox map.
Step-by-Step Explanation:
1.	Removing getIso(); Call: First, you remove the getIso(); call from the bottom of your JavaScript. This is the previous function responsible for fetching the isochrone data from the API and logging it to the console. Since you're replacing this step with code to display the contour on the map, this call is no longer needed.
2.	Set Up a New Source: You need to define a source that will provide the isochrone data to Mapbox GL JS. The source acts as a data feed for the map. Here's the code to set up a new source:
javascriptCopy code
map.addSource('isochrone', { type: 'geojson', data: { type: 'Feature', properties: {}, geometry: { type: 'Polygon', coordinates: [/* Add your coordinates here */] } } }); 
•	In the code, map.addSource('isochrone', ...) creates a new source named 'isochrone' in your map.
•	The type: 'geojson' specifies that the source will provide GeoJSON data, which is a common format for geographic data.
•	The data property defines the actual data to be displayed on the map. It specifies the type, properties, and geometry of the GeoJSON feature.
3.	Set Up a New Layer: Next, you'll create a layer that defines how the isochrone data should be visually represented on the map. Here's the code:
javascriptCopy code
map.addLayer({ id: 'isochrone-layer', type: 'fill', source: 'isochrone', layout: {}, paint: { 'fill-color': '#007cbf', 'fill-opacity': 0.5 } }); 
•	In this code, map.addLayer(...) is used to create a new layer named 'isochrone-layer' that is linked to the 'isochrone' source.
•	The type: 'fill' specifies that this layer should be a filled polygon.
•	The paint property sets the visual appearance of the isochrone contour. It defines the fill color ('fill-color') and opacity ('fill-opacity') of the polygon.
Finishing Up:
Once you've replaced the getIso(); call with these new source and layer definitions, your map will be ready to display the isochrone contour based on the provided data. This contour will be styled with the specified fill color and opacity, making it visible and interactive for users.
Background:
In the previous steps, you set up a source and a layer for displaying the isochrone contour on the map. Now, you're connecting the data fetched from the Isochrone API to the map source. This means that the isochrone contour will be drawn on the map based on the data you receive from the API.
Step-by-Step Explanation:
1.	Replace console.log(data): In your getIso function, you have a console.log(data) statement. This statement was previously used to view the API response data in the browser's console. Since you've successfully fetched the isochrone data, you're now going to use this data to draw the contour on the map.
2.	Set the 'iso' Source's Data: The code map.getSource('iso').setData(data); is used to set the data for the source named 'iso.' This source is the one you created in a previous step to provide the isochrone data to the map.
•	map.getSource('iso') refers to the source you've defined for the isochrone data.
•	.setData(data) is a method to update the data for this source. In this case, you're setting it to the data returned by the API query.
What This Code Does:
By replacing the console.log(data) statement with this code, you're instructing Mapbox GL JS to update the 'iso' source with the isochrone data you receive from the API. As a result, the isochrone contour will be drawn on the map according to the specified parameters, such as the cycling routing profile and a trip duration of 10 minutes.
Testing:
After saving your changes and refreshing the page in your browser, you should see the isochrone contour displayed on the map. The contour will represent the area that can be reached within 10 minutes by cycling based on your hardcoded parameters.
 
Background:
In the earlier steps, you set up the map, created a sidebar with buttons for users to select transportation profiles and time durations, integrated the Isochrone API to fetch data for isochrone contours, and added code to draw the initial contours on the map. Now, you're adding further functionality to allow dynamic changes based on user interactions.
Step-by-Step Explanation:
1.	Selecting HTML Element:
javascriptCopy code
const params = document.getElementById('params'); 
•	This line selects the HTML form element with the ID "params." The form contains the buttons that users click to change the routing profile or trip duration.
2.	Event Listener:
javascriptCopy code
params.addEventListener('change', (event) => { ... }); 
•	An event listener is added to the "params" form. The event to listen for is the "change" event, which occurs when a user interacts with the form's input elements (buttons).
3.	Event Handling Function:
•	Inside the event listener function, there's logic to respond when a user changes either the profile (e.g., walking, cycling, driving) or the duration (e.g., 10 minutes, 20 minutes, 30 minutes). The function is triggered when the user interacts with the buttons.
•	if (event.target.name === 'profile') { ... }: This checks if the user changed the profile (transportation mode) by clicking one of the buttons. If so, it updates the profile variable with the selected profile value.
•	else if (event.target.name === 'duration') { ... }: This checks if the user changed the trip duration by clicking one of the buttons. If so, it updates the minutes variable with the selected duration value.
4.	Fetching Updated Data:
•	After updating the profile or minutes variable, the getIso() function is called. This function sends a new fetch request to the Isochrone API with the updated parameters.
5.	Result of Interaction:
•	When a user clicks on different combinations of routing profiles and trip durations, the event listener reacts by updating the profile and minutes variables. Then, it makes a new API query using the updated values. The API response is used to redraw the isochrone contours on the map.
What This Code Does:
This code enables your web app to respond to user interactions. When a user clicks on a button to change the routing profile or trip duration, the event listener updates the relevant variable and triggers a new API query to fetch updated isochrone data. As a result, the map displays isochrone contours based on the user's selections.
Testing:
After adding this code and saving your changes, you should be able to interact with the buttons on the sidebar. Click on different combinations of routing profiles and trip durations, and observe how the isochrone contours change on the map in real-time. This step enhances the interactivity of your web app.
 
Step-by-Step Explanation:
1.	Creating a Marker:
javascriptCopy code
const marker = new mapboxgl.Marker({ color: '#314ccd' }); 
•	This code initializes a new Mapbox GL JS marker. The color property sets the color of the marker. In this case, it's set to a shade of blue (#314ccd).
2.	Defining Coordinate Object:
javascriptCopy code
const lngLat = { lon: lon, lat: lat }; 
•	Here, you create a JavaScript object called lngLat to store the coordinates (longitude and latitude) where you want to place the marker. These coordinates should match the location around which you generated the isochrone.
3.	Adding Marker to Map:
•	To add the marker to the map, you need to place it on the map when it loads. This is done within the map.on('load') function.
javascriptCopy code
// Initialize the marker at the query coordinates marker.setLngLat(lngLat).addTo(map); 
•	The setLngLat() method is used to specify the coordinates where you want the marker to appear. In this case, you use the lngLat object created earlier. The addTo(map) method attaches the marker to the map, making it visible.
What This Code Does:
This code adds a marker to the map at the specified coordinates. The marker is a blue dot that stands out against the map background. By placing the marker at the query coordinates, you provide a clear reference point, allowing users to identify the central location of the isochrone contour.
Testing:
After adding this code, save your changes and refresh the page. When you reload the web app, you will see a blue marker on the map at the specified coordinates (in this case, the Mapbox office in Washington D.C.). The marker makes the center of the isochrone contour more distinct and visually enhances your web app.
