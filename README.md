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

### Travelling Salesman Problem (TSP) Solver - `tsp-solver` repository



### Visualization - `front_end` repository
Helps in visualising the output of tsp solver on map built with node express and uses mapbox to plot maps. It plots the output json which contains the step-wise route of all the vehicles.


