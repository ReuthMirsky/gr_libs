# Domains and Algorithms

This page contains the domain descriptions, supported environments, and algorithm.

## Supported Gym Environments

| **Environment Class**  | **Action Space** | **State Space** | **Package Name** |
|------------|----------------|----------------|----------------|
| Minigrid   | Discrete       | Discrete       | minigrid           |
| PointMaze  | Continuous     | Continuous     | gymnasium-robotics |
| Parking    | Continuous     | Continuous     | highway-env        |
| Panda      | Continuous     | Continuous     | panda-gym          |

## Supported Environment Instances

| **Environment Class**  | **Specific Environment ID**                | **GC Adaptable** | **Agent Demo**                                   |
|-----------------------|---------------------------------------------|------------------|--------------------------------------------------|
| Minigrid              | MiniGrid-SimpleCrossingS13N4                | ❌               | <img src="gifs/minigrid_walls.png" width="120"/> |
| Minigrid              | MiniGrid-LavaCrossingS9N2                   | ❌               | <img src="gifs/minigrid_lava.png" width="120"/>  |
| PointMaze             | PointMaze-FourRoomsEnvDense-11x11           | ✅               | <img src="gifs/point_maze.gif" width="120"/>     |
| PointMaze             | PointMaze-ObstaclesEnvDense-11x11           | ✅               | <img src="gifs/point_maze_obstacles.gif" width="120"/> |
| Parking               | Parking-S-14-PC-                            | ✅               | <img src="gifs/parking.gif" width="120"/>        |
| Panda                 | PandaMyReachDense                           | ✅               | <img src="gifs/panda.gif" width="120"/>          |

> **Note:** If viewing on GitHub, GIFs may not animate in the table preview. For best results, view the README in a Markdown viewer that supports HTML tags.

Do note one can create other environments outside the supported environments, but they're not a part of the benchmark.

The demos were produced by running odgr_executor.py with a '--collect_stats' flag.

## Supported Algorithms

| **Algorithm**        | **Supervised** | **Reinforcement Learning** | **Discrete States** | **Continuous States** | **Discrete Actions** | **Continuous Actions** | **Model-Based** | **Model-Free** | **Action-Only** | **Goal Conditioned** | **Fine-Tuning** | **Supported Environments**                |
|---------------------|----------------|---------------------------|---------------------|----------------------|----------------------|-----------------------|------------------|----------------|----------------|---------------------|-----------------|-------------------------------------------|
| Graql               | ❌             | ✅                        | ✅                  | ❌                   | ✅                   | ❌                    | ❌               | ✅             | ❌             | ❌                  | ❌              | Minigrid                                   |
| Draco               | ❌             | ✅                        | ✅                  | ✅                   | ✅                   | ✅                    | ❌               | ✅             | ❌             | ❌                  | ❌              | MinigridSimple, MinigridLava, PointMazeObstacles, PointMazeFourRooms, PandaReach, Parking            |
| GCDraco             | ❌             | ✅                        | ✅                  | ✅                   | ✅                   | ✅                    | ❌               | ✅             | ❌             | ✅                  | ❌              | PointMazeObstacles, PointMazeFourRooms, PandaReach, Parking                       |
| GCAura              | ❌             | ✅                        | ✅                  | ✅                   | ✅                    | ❌               | ✅             | ❌             | ✅                  | ✅              | ✅              | PointMaze, PandaReach, Parking            |
| ExpertBasedGraml    | ✅             | ✅                        | ✅                  | ✅                   | ✅                   | ✅                    | ❌               | ✅             | ✅             | ❌                  | ❌              | MinigridSimple, MinigridLava, PointMazeObstacles, PointMazeFourRooms, PandaReach, Parking                       |
| GCGraml             | ✅             | ✅                        | ✅                  | ✅                   | ✅                   | ✅                    | ❌               | ✅             | ✅             | ✅                  | ❌              | PointMazeObstacles, PointMazeFourRooms, PandaReach, Parking                       |

