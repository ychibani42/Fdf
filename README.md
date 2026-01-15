# FDF - Fil de Fer (Wireframe)

A 3D wireframe renderer that transforms 2D heightmap data into isometric 3D projections using the MiniLibX graphics library.

## 📝 Description

FDF (Fil de Fer, meaning "wireframe" in French) is a graphics project that reads heightmap data from `.fdf` files and renders them as 3D wireframe models in isometric projection. The program visualizes terrain or elevation data by connecting points in a grid, where each value represents the z-coordinate (height) of a point in 3D space.

## ✨ Features

- **Isometric Projection**: Transforms 3D coordinates into 2D isometric view
- **Dynamic Rendering**: Uses Bresenham's line algorithm for efficient line drawing
- **Color Mapping**: Heights are colored differently (elevated points in red, ground level in white)
- **Interactive Window**: MLX-based graphical window with keyboard controls
- **Automatic Scaling**: Adapts zoom and scaling based on map dimensions
- **Error Handling**: Validates input files and handles parsing errors

## 🛠️ Technical Details

### Key Components

- **Graphics Library**: MiniLibX (MLX) for X11 window management and rendering
- **Language**: C
- **Compiler**: Clang with `-Wall -Werror -Wextra` flags
- **Dependencies**: libft (custom C library), MLX, X11, Xext, math library

### Core Algorithms

1. **Isometric Projection**: 
   - X-axis: `(x - y) * cos(0.6) * zoom + screen_width/2`
   - Y-axis: `(x + y) * sin(0.6) * zoom - z * (zoom/scaling) + screen_height/2`

2. **Line Drawing**: Bresenham's algorithm for rasterizing lines between grid points

3. **Color Assignment**:
   - Elevated points (z > 0): Red (`0xFFFF0000`)
   - Ground level (z = 0): White (`0xFFFFFFFF`)

## 📦 Installation

### Prerequisites

- Linux operating system
- X11 development libraries
- Make build system
- Clang compiler

### Build Instructions

```bash
# Clone the repository
git clone https://github.com/ychibani42/__fdf.git
cd __fdf

# Compile the project
make

# This will:
# 1. Compile libft library
# 2. Compile MLX library
# 3. Build the fdf executable
```

### Clean Commands

```bash
make clean   # Remove object files
make fclean  # Remove object files and executable
make re      # Rebuild everything from scratch
```

## 🚀 Usage

### Running the Program

```bash
./fdf <map_file.fdf>
```

### Map File Format

FDF files contain space-separated integer values representing elevation/height:

```
0 0 0 0 0
0 1 2 1 0
0 2 3 2 0
0 1 2 1 0
0 0 0 0 0
```

Each number represents the z-coordinate (height) at that grid position.

### Controls

- **ESC**: Close the window and exit the program
- **X button**: Close the window (standard window close)

## 📂 Project Structure

```
__fdf/
├── Makefile              # Build configuration
├── README.md            # This file
├── includes/            # Header files
│   ├── fdf.h           # Main header
│   ├── fdf_structs.h   # Structure definitions
│   ├── fdf_defines.h   # Constants and key codes
│   └── fdf_fonctions.h # Function prototypes
├── srcs/               # Source files
│   ├── fdf/           
│   │   └── main.c      # Entry point and MLX loop
│   ├── parsing/       
│   │   └── parsing.c   # File validation and parsing
│   ├── init/          
│   │   ├── init.c          # Main initialization
│   │   ├── init_map_data.c # Map metadata initialization
│   │   ├── init_mlx.c      # MLX initialization
│   │   └── init_points.c   # 3D grid point initialization
│   ├── utils/         
│   │   ├── init_utils.c      # Helper utilities
│   │   ├── mlx_utils.c       # MLX helper functions
│   │   ├── mapping_utils.c   # Isometric projection
│   │   └── line_algorithm.c  # Bresenham's algorithm
│   ├── events/        
│   │   └── events.c    # Keyboard and window events
│   └── clean/         
│       └── clean_quit.c # Memory cleanup
├── libft/              # Custom C library
└── mlx/                # MiniLibX graphics library
```

## 🔧 Key Data Structures

### t_3d - 3D Point
```c
typedef struct s_3d {
    int x;              // X coordinate
    int y;              // Y coordinate
    int z;              // Z coordinate (height)
    unsigned int color; // Point color
} t_3d;
```

### t_2d - 2D Projected Point
```c
typedef struct s_2d {
    int x;     // Screen X coordinate
    int y;     // Screen Y coordinate
    int color; // Point color
} t_2d;
```

### t_map_data - Map Metadata
```c
typedef struct s_map_data {
    int x_size;    // Map width
    int y_size;    // Map height
    int tot_size;  // Total points
    char *file_name; // Input file path
    int fd;        // File descriptor
    int zoom;      // Zoom factor
    int scaling;   // Z-axis scaling
    int min;       // Minimum Z value
    int max;       // Maximum Z value
} t_map_data;
```

### t_program_data - Main Program State
```c
typedef struct s_program_data {
    int fd;             // File descriptor
    char *file_name;    // Input file
    t_fdf *fdf;        // MLX data
    t_line *line;      // Line drawing data
    t_3d **grid;       // 3D grid points
    t_2d **final_map;  // 2D projected points
    t_map_data *map_data; // Map metadata
} t_program_data;
```

## 🎯 Program Flow

1. **Validation**: Check command-line arguments and validate `.fdf` file
2. **Parsing**: Read file to determine map dimensions (x_size, y_size)
3. **Initialization**:
   - Allocate memory for 3D grid
   - Read heightmap values from file
   - Initialize MLX window and image buffer
   - Calculate zoom and scaling factors
4. **Projection**: Apply isometric transformation to convert 3D to 2D
5. **Rendering**: 
   - Draw lines between adjacent grid points using Bresenham's algorithm
   - Put image to window
6. **Event Loop**: Handle keyboard input and window events
7. **Cleanup**: Free all allocated memory and destroy MLX resources

## 🎨 Display Settings

- **Screen Resolution**: 1920x1080
- **Default Scaling**: 10 (for Z-axis)
- **Projection Angle**: 0.6 radians (~34.4°)
- **Auto-centering**: Map is centered on screen

## 🧮 Mathematical Concepts

### Isometric Projection Matrix
The program uses a simplified isometric projection:
- Rotation around X-axis by 0.6 radians
- Scaling based on zoom factor
- Translation to center of screen

### Bresenham's Line Algorithm
Efficient integer-only algorithm for drawing lines:
- No floating-point operations
- Incremental error calculation
- Octant-based direction handling

## 📜 License

This is a student project from 42 School.

## 👤 Author

**ychibani** (ychibani@student.42.fr)

## 🔗 Resources

- [42 School](https://www.42.fr/)
- [MiniLibX Documentation](https://harm-smits.github.io/42docs/libs/minilibx)
- [Bresenham's Line Algorithm](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm)
- [Isometric Projection](https://en.wikipedia.org/wiki/Isometric_projection)
