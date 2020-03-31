# Meraki-mishmash

## Implementation Workflow
```
Sample Dataset (jobs.csv + vehicles.csv) -> problem_setter.py -> input_to_engine.json -> TSP_Solver -> Output.json -> upload to front end -> Visualisation
```

### Sample Dataset
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

### Distance Matrix API - `osrm_india_map` repository
Builds a docker image of OSRM with India's latest map loaded inside the image. Thus, everytime we do not have to download map of india, which is a big file. It acts as a pre-warmed container.

Create a dir for building an images
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

### Travelling Salesman Problem (TSP) Solver - `tsp-solver` repository



### Visualization - `front_end` repository
Helps in visualising the output of tsp solver on map built with node express and uses mapbox to plot maps. It plots the output json which contains the step-wise route of all the vehicles.


*Yowza, we enrolled on 29th march and got our Azure pass on 31st evening, we could not deploy the front end. If qualified we will demo the front end visualization and solution deployed of azure :D !*


