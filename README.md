# refactoring

## Game of Life

from Kongtapp's PA4 The Game of Life: [https://github.com/KongtappV/pa4-KongtappV](https://github.com/KongtappV/pa4-KongtappV)

## printBoard() and countNeightbors Method

these two method in `src/application/GameOfLife.java/` should be in `src/application/Board.java/` since these two method is related to Board directly

```Java
/**
 * Print the board in the console for observation and testing
 */
public void printBoard() {
    for (int y = 0; y < board.getRows(); y++) {
        StringBuilder line = new StringBuilder("|");
        for (int x = 0; x < board.getColumns(); x++) {
            if (board.getValue(x, y) == board.DEAD) {
                line.append(" 0 |");
            } else {
                line.append(" 1 |");
            }
        }
        System.out.println(line);
    }
    System.out.println();
}

/**
 * Return number of ALIVE neighbors around the given coordinate
 * 
 * @param x horizontal coordinate
 * @param y vertical coordinate
 * @return number of ALIVE neighbors
 */
public int countNeighbors(int x, int y) {
    // position of the surrounding neighbors
    int[][] position = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    int count = 0;
    for (int[] set : position) {
        count += board.getValue(x + set[0], y + set[1]);
    }
    return count;
}
```

* Refactoring Signs:
  these two method is used data from `Board` so these must be put in the wrong object
* Refactoring:
  1. moved these method to `Board.java`
  1. Change `Board` reference in code to `self`
  1. also countNeighbors is missing the Javadoc
  
 after refactoring:
 
 ```Java
public class Board {
    ...
    
    /**
     * Print the board in the console for observation and testing
     */
    public void printBoard() {
        for (int y = 0; y < board.getRows(); y++) {
            StringBuilder line = new StringBuilder("|");
            for (int x = 0; x < board.getColumns(); x++) {
                if (self.getValue(x, y) == self.DEAD) {
                    line.append(" 0 |");
                } else {
                    line.append(" 1 |");
                }
            }
            System.out.println(line);
        }
        System.out.println();
    }
    
    /**
     * Return number of ALIVE neighbors around the given coordinate
     * 
     * @param x horizontal coordinate
     * @param y vertical coordinate
     * @return number of ALIVE neighbors
     */
    public int countNeighbors(int x, int y) {
        // position of the surrounding neighbors
        int[][] position = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
        int count = 0;
        for (int[] set : position) {
            count += self.getValue(x + set[0], y + set[1]);
        }
        return count;
    }
}
```

in `GameOfLife.java` to keep it simple inside the same method we just move we can just call the same function from `Board`.

```Java
    /**
     * Print the board in the console for observation and testing
     */
    public void printBoard() {
        board.printBoard();
    }
    
    /**
     * Return number of ALIVE neighbors around the given coordinate
     * 
     * @param x horizontal coordinate
     * @param y vertical coordinate
     * @return number of ALIVE neighbors
     */
    public int countNeighbors(int x, int y) {
        return board.countNeighbors(x, y);
    }
```

## GOLApp.java

This files is a really big mess what I should have done is separated `initComponent()` in to `class` in different files instead of put everything into one big file.
`GOLAPP.java` should only have these function.

```Java
public class GOLApp extends Application {
	@Override
	public void start(Stage primaryStage) {
		try {
			FlowPane root = initComponent();
			primaryStage.setTitle("The Game of Life");
			gol.random();
			primaryStage.setScene(new Scene(root));
			primaryStage.show();
			draw(gol.board);
			
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) {
		launch(args);
	}
}
```

