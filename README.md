## Meraki - 1st Runner's Up in Skillenza MishMash 2020 Hack :trophy: :tada:
[![Docker](https://img.shields.io/badge/Docker-Build-Suc)](https://github.com/festivitymishra/Meraki-mishmash)
### Ideation

Fast Route Planning & Route Optimisation with the help of travelling salesman problem (TSP) algorithm - Given a set of deliveries to be made in particular time duration, the TSP Algorithm computes the fastest and shortest route for the given fleet of vehicles. If a fleet of vehicles is to deliver consignments till the last mile, the TSP algorithm assigns route to each vehicle so that the total transportation costs are minimized and can also consider business-side logics such as the capacity of each vehicle, time-windows, delivery skills and more. For the shortest and fastest route calculation, the TSP solver (VROOM) relies on OpenStreetMap data and does matrix calculations by triggering Open-Source Routing Machine (OSRM), thus making the route locality-aware.

![IDEA](resources/Idea.JPG?raw=true "Ideation")

*****************************************************
### High Level Architecture Diagram (Final Deployment Proposed)

![Architecture](resources/MISHMASH.png?raw=true "Architecture")

*****************************************************
### Sample Dataset (Self Generated)
The dataset considered for the **Route Optimization and Visualization for Sales Vehicles** problem statement is as follows:


| Headers in `jobs.csv`  | Headers in `vehicles.csv` |
| ------------- | ------------- |
| Customer  | Vehicle  |
| Job ID  | Vehicle ID  |
| Pickup ID  | Capacity  |
| Drop ID  | Latitude  |
| Pickup Latitude  | Longitude  |
| Pickup Longitude  |   |
| Drop Latitude  |   |
| Drop Longitude  |   |
| Pickup Time  |   |
| Drop Time  |   |
| Abs Time  |   |
| Drop Off Time Taken (10 min)  |   |
| Pickup Time Taken (45 Mins)  |   |
| Weight  |   |

##### Use problem_setter.py file to conver csv to input.json, Azure Functions to be used as TSP Problem Setters.

*****************************************************
### Implementation Workflow
```
Sample Dataset (jobs.csv + vehicles.csv) -> problem_setter.py -> input_to_engine.json -> TSP_Solver -> Output.json -> upload to front end -> Visualisation
```
*****************************************************

### Distance Matrix API - `osrm_india_map` repository
Builds a docker image of OSRM with India's latest map loaded inside the image. Thus, everytime we do not have to download map of india, which is a big file. It acts as a pre-warmed container.

Create a dir for building an docker image
```
mkdir osrm_india_map
cd osrm_india_map
```

copy Dockerfile here or create a new file and paste the content in it.
```
nano Dockerfile 
```

Lets make the image
```
docker build -t osrm_india_map .
```

Lets run the container
```
docker run -p 5000:5000 osrm_india_map
```

Test if it is working
```
curl "http://127.0.0.1:5000/route/v1/driving/13.388860,52.517037;13.385983,52.496891?steps=true"
```
##### Now Stop the docker image, we need the ports to be free for final orchestration :yum:
*****************************************************

### VROOM - Travelling Salesman Problem (TSP) Solver - `tsp-solver` repository
##### Unzip the folder `vroom-feature-pickup-and-delivery.zip` in same dir with name 'vroom-feature-pickup-and-delivery'

create app folder
```
mkdir tsp_solver 
```

lets go into the folder
```
cd tsp_solver 
```

create a file called Dockerfile and paste the contents of Dockerfile or simple put the file
```
nano Dockerfile -> Then Paste content
```

do the same for config.js file
```
nano config.js 
```

Lets build image
```
docker build -t tsp_solver .
```

Lets run the image
```
docker run -p 3000:3000 tsp_solver
```

check if its running (first make sure "osrm_india_map" container is running)
```
curl 'http://localhost:3000/' -H 'Content-type: application/json' --data-binary \
'{ "jobs": [ { "id": 1613, "service": 1200, "amount": [ 1 ], "location": [ 29.02988, 40.99423 ] }, { "id": 1665, "service": 1200, "amount": [ 1 ], "location": [ 29.216, 41.008520000000004 ] }, { "id": 21234, "service": 900, "amount": [ 1 ], "location": [ 29.272640000000003, 40.94765 ] }, { "id": 23457, "service": 600, "amount": [ 1 ], "location": [ 29.119659999999996, 40.97359 ] }, { "id": 24145, "service": 900, "amount": [ 1 ], "location": [ 29.16579, 40.925540000000005 ] }, { "id": 33007, "service": 900, "amount": [ 1 ], "location": [ 29.123801, 40.978068 ] }, { "id": 38081, "service": 600, "amount": [ 1 ], "location": [ 29.113429999999997, 40.980259999999994 ] }, { "id": 39163, "service": 900, "amount": [ 1 ], "location": [ 29.25528, 40.87539 ] } ], "vehicles": [ { "id": 7, "start": [ 29.208498, 40.890795 ], "end": [ 29.208498, 40.890795 ], "capacity": [ 25 ], "time_window": [ 30600, 61200 ], "startDescription": "Start", "endDescription": "End" } ], "options": { "g": true }}' --compressed
```
##### Now Stop the docker image, we need the ports to be free for final orchestration.

*****************************************************
### Visualization - `front_end` repository
Helps in visualising the output of tsp solver on map built with node express and uses mapbox javascript to plot maps. It plots the output json which contains the step-wise route of all the vehicles.

Create a dir for building an image
```
mkdir front_end
cd front_end
```

copy Dockerfile here or create a new file and paste the content in it.
```
nano Dockerfile 
```

Lets make the image
```
docker build -t front_end .
```

Lets run the container
```
docker run -p 9966:9966 front_end
```
##### Now Stop the docker image, we need the ports to be free for final orchestration.
*****************************************************

### Container orchestration with Docker-Compose
#### Note this can be easily performed with **Azure kubernetes Service** or AWS Elastic Contaainer Service
Come to the main dir of project.

```
docker-compose up -d
```
#### wait for the magic to happen. Once built go to [Localhost](127.0.0.1:9966/) :innocent:

*****************************************************

## Services needed to be deployed on Azure:

- [ ] Azure Cosmos DB - Change Feed
- [ ] Azure Container Instances
- [ ] Azure Container Registry
- [ ] Azure kubernetes Service
- [ ] Azure Sever-less Functions
- [ ] Azure Machine Learning AutoML
- [ ] Azure Document DB
- [ ] Bing Maps - Distance Matrix API
- [ ] Microsoft Power BI - Front End

Please make sure to update tests as appropriate.

*****************************************************
## Notes

#### License 
##### [Apache License 2.0](https://github.com/festivitymishra/Meraki-mishmash/blob/master/LICENSE)

If there is anything totally unclear, or not working, please feel free to file an issue.
 Reach out at to contributors :- 

[Kshitija](kshitija.iitkgp@gmail.com) | [Utsav](utsav.iitkgp@gmail.com)

If this project was helpful for you please show some love :star:

##### :whale: Docker build, for social distancing! #StayHome #StaySafe #Covid-19
