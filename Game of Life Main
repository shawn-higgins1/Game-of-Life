/***********************
*Game of Life program  *
***********************/ 

#include <allegro.h>
#include <apmatrix.h>
#include <iostream>

using namespace std;

#define COLUMNS 100
#define ROWS 50
#define SQUARE_SIZE 10
#define COLOUR_ALIVE  makecol(0,255,0)  
#define COLOUR_DEAD   makecol(200,200,200)   
#define MOUSE_OVER   makecol(0,0,0)

//prototyping
void nextGeneration(apmatrix <bool> &grid);

int main(void) {
	apmatrix<bool> grid(ROWS, COLUMNS);
	int colour;
    int x = 0;
    int y = 0;
    int mouse_x_over = 0;
    int mouse_y_over = 0;
	int generation = 0;
	
	srand(time(NULL));

    //allegro initialization
    allegro_init(); 
    install_keyboard(); 
    install_timer();
    install_mouse();
    
    set_color_depth(32);

    //initialize video mode to 640x480
    if (set_gfx_mode(GFX_AUTODETECT_WINDOWED, COLUMNS*SQUARE_SIZE, 
     ROWS*SQUARE_SIZE, 0, 0)) {
        allegro_message(allegro_error);
        return 1;
    }
    
    set_window_title("Game of Life");
    
    show_mouse(screen);
    
    BITMAP *buffer = NULL;
    
    //creates a buffer
   	buffer = create_bitmap(COLUMNS*SQUARE_SIZE, ROWS*SQUARE_SIZE);
	if (!buffer){
		return 1;
	}
    
    //intiliazes all the values in the matrix to false
    for(int i = 0; i < ROWS; i++){
		for(int j = 0; j < COLUMNS; j++){
			grid[i][j] = false;
		}
	}
  	
	//Allows to the user to select which cells are alive by
	//clicking on each individual cell with the mouse		     
	while(!key[KEY_ENTER]){
		clear(buffer);
		
		//fills in the buffer in accordance to the values in
		//the matrix
		for (int i = 0; i < ROWS; i++) {
			for (int j = 0; j < COLUMNS; j++) {
				x = i * SQUARE_SIZE;
    			y = j * SQUARE_SIZE;
    		    colour = grid[i][j] ? COLOUR_ALIVE : COLOUR_DEAD;
    		    rectfill(buffer, y, x, y+SQUARE_SIZE, x+SQUARE_SIZE, colour);
 			} 		
     	}
     	
     	//prints instructions to the screen
     	textprintf_ex(buffer, font, 0, 0, makecol(255,0,0), -1, 
		 "Press enter to start."); 
		textprintf_ex(buffer, font, 0, 15, makecol(255,0,0), -1,
		"Left click to select. Right click to deselect.");
  		
  		//calculates what grid cell the mouse is in
		mouse_x_over = mouse_x / SQUARE_SIZE;
		mouse_y_over = mouse_y /SQUARE_SIZE;
  		
  		//creates a live cell where the mouse is located
		if( mouse_b == 1)	{
		  	grid[mouse_y_over][mouse_x_over] = true;
		}
		
		//creates a dead cell where the mouse is located  
		if( mouse_b == 2)	{
		  	grid[mouse_y_over][mouse_x_over] = false;
		}     
  		
  		//shades in the grid cell that the mouse is loacted in
  		rectfill(buffer, mouse_x_over*SQUARE_SIZE, mouse_y_over*SQUARE_SIZE, 
		  (mouse_x_over++)*SQUARE_SIZE, (mouse_y_over++)*SQUARE_SIZE, MOUSE_OVER);
  		 
  		blit(buffer, screen, 0, 0, 0, 0, COLUMNS*SQUARE_SIZE, ROWS*SQUARE_SIZE);
	}
	
	//keepings creating new generations until the escape key is pressed
    while(!key[KEY_ESC]) {
	   	clear(buffer);
	   	
	   	//this converts the information from the matrix into a bitmap which is then displayed
       	for (int i = 0; i < ROWS; i++) {
			for (int j = 0; j < COLUMNS; j++) {
				x = i * SQUARE_SIZE;
    			y = j * SQUARE_SIZE;
    		    colour = grid[i][j] ? COLOUR_ALIVE : COLOUR_DEAD;
    		    rectfill(buffer, y, x, y+SQUARE_SIZE, x+SQUARE_SIZE, colour);
 			} 		
     	}
     	
     	//displays information about the generation on the screen
     	textprintf_ex(buffer, font, 0, 0, 15, -1,
  			     "Generation- %d - Press ESC to quit",   generation++);
  			     
  		blit(buffer, screen, 0, 0, 0, 0, COLUMNS*SQUARE_SIZE, ROWS*SQUARE_SIZE);	     
        rest(500); 
		 
        //determines what the grid for the next generation should be
     	nextGeneration(grid);
    
    }   
	//end program
    allegro_exit();
    return 0;
}
END_OF_MAIN();

void nextGeneration(apmatrix <bool> &grid){
	apmatrix <bool> tempGrid(ROWS,COLUMNS);
	int neighbours = 0;
	
	//this determines whether a each cell is alive or dead
	for(int i = 0; i < ROWS; i++){
		for(int j = 0; j < COLUMNS ; j++){
			//This determines the number of neighbours a cell has
			if(j != (COLUMNS - 1)){
				if(grid[i][j + 1]){
					neighbours ++;
				}
			}
			if(j != 0){
				if(grid[i][j - 1]){
					neighbours ++;
				}
			}
			if(i != 0){
				if(grid[i-1][j]){
					neighbours ++;
				}
				if(j != (COLUMNS - 1)){
					if(grid[i -1][j + 1]){
						neighbours ++;
					}
				}
				if(j != 0){
					if(grid[i -1][j - 1]){
						neighbours ++;
					}
				}
			}
			if(i != (ROWS - 1)){
				if(grid[i + 1][j]){
					neighbours ++;
				}
				if(j != 0){
					if(grid[i + 1][j-1]){
						neighbours ++;
					}
				}
				if(j != (COLUMNS - 1)){
					if(grid[i + 1][j + 1]){
						neighbours ++;
					}
				}
			}
			
			//this determines whether a cell is alive or dead given the number of neighbours
			if(neighbours == 2){
				tempGrid[i][j] = grid[i][j];
			}
			else if(neighbours == 3){
				tempGrid[i][j] = true;
			}
			else{
				tempGrid[i][j] = false;
			}
			neighbours = 0;
		}
	}
	
	grid = tempGrid;
}
END_OF_FUNCTION(nextGeneration);
