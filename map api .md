# Documentation regarding google map api use case

## Details extraction 
The Places API has a robust set of data that can be retrieved and displayed on the map for a particular place using the getDetails() method. In this example, a Google office is displayed with a pin on the map. The name, unique placeID and address for this place are displayed when clicking on the marker.

Example data fields that can be retrieved and displayed include: address, name, photo, type, phone number, website, opening hours, user ratings. To get a detailed list of details available for display, view this list . It is recommended to choose what parameters you want to display instead of showing it all to optimise cost

``` javascript
<!DOCTYPE html>
<!--
 @license
 Copyright 2019 Google LLC. All Rights Reserved.
 SPDX-License-Identifier: Apache-2.0
-->
<html>
  <head>
    <title>Place Details</title>
    <script>
      /**
       * @license
       * Copyright 2019 Google LLC. All Rights Reserved.
       * SPDX-License-Identifier: Apache-2.0
       */
      // This example requires the Places library. Include the libraries=places
      // parameter when you first load the API. For example:
      // <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places">
      function initMap() {
        const map = new google.maps.Map(document.getElementById("map"), {
          center: { lat: -33.866, lng: 151.196 },
          zoom: 15,
        });
        const request = {
          placeId: "ChIJN1t_tDeuEmsRUsoyG83frY4",
          fields: ["name", "formatted_address", "place_id", "geometry"],
        };
        const infowindow = new google.maps.InfoWindow();
        const service = new google.maps.places.PlacesService(map);

        service.getDetails(request, (place, status) => {
          if (
            status === google.maps.places.PlacesServiceStatus.OK &&
            place &&
            place.geometry &&
            place.geometry.location
          ) {
            const marker = new google.maps.Marker({
              map,
              position: place.geometry.location,
              title: place.name,
            });

            google.maps.event.addListener(marker, "click", () => {
              const content = document.createElement("div");
              const nameElement = document.createElement("h2");

              nameElement.textContent = place.name;
              content.appendChild(nameElement);

              const placeIdElement = document.createElement("p");

              placeIdElement.textContent = place.place_id;
              content.appendChild(placeIdElement);

              const placeAddressElement = document.createElement("p");

              placeAddressElement.textContent = place.formatted_address;
              content.appendChild(placeAddressElement);
              infowindow.setContent(content);
              infowindow.setOptions({ariaLabel: place.name});
              infowindow.open(map, marker);
            });
          }
        });
      }

      window.initMap = initMap;
    </script>
    <style>
      /**
       * @license
       * Copyright 2019 Google LLC. All Rights Reserved.
       * SPDX-License-Identifier: Apache-2.0
       */
      /* 
       * Always set the map height explicitly to define the size of the div element
       * that contains the map. 
       */
      #map {
        height: 100%;
      }

      /* Optional: Makes the sample page fill the window. */
      html,
      body {
        height: 100%;
        margin: 0;
        padding: 0;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
    <script
      src="https://maps.googleapis.com/maps/api/js?key=INSERT_YOUR_API_KEY&callback=initMap&libraries=places&v=weekly&solution_channel=GMP_CCS_placedetails_v2"
      defer
    ></script>
  </body>
</html>
```
