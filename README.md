# pa1

using System;
using static System.Console;

namespace pa1
{
    static class Program
    {

    
        static string[ ] letters = { "a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l" };
        static int boardSize = 8; // must be in the range 1..12.

        
        static double cellMarkProb = 0.2;
        static Random rGen = new Random( );

        
        
        static void Main( )
        {
            
        
            // initializes and allocates 2 boards: a target or 'hidden' board
            // and the board which the user can view and manipulate with the aim of matching
            // the hidden board
        
            bool[,]hiddenBoard = new bool[boardSize, boardSize];
            bool[,] userBoard = new bool [boardSize, boardSize];
    
            // the following arrays will store the sums for the hidden board's rows and columns 
            // that are marked (with an X)
    
            int [] sumRow = new int[boardSize];
            int [] sumColumn = new int [boardSize];
            
            // the following arrays  will store the sums for the user board's rows and columns 
            // that are marked (with an X)
            //
            int [] userSumColumn = new int [boardSize];
            int [] userSumRow = new int [boardSize];
            
        
            //initializes the sums of all rows and colums to 0
            //
            for (int i= 0; i < boardSize; i++) 
            {
            
            sumRow[i] = 0;
            sumColumn[i] = 0;
            userSumRow[i] = 0;
            userSumColumn[i] = 0;
            
            }
            
            // initializes all bool values in hidden board and user to false (no marked cells)
            
            for (int row = 0; row < boardSize; row++) 
            {
                    
                for (int col = 0; col < boardSize; col++)
                {
                
                hiddenBoard[row,col] = false;
                userBoard[row,col] = false;
                    
                }
            }
                
                
            // if the random generated probability is less or equal to 0.2 the
            // bool value in that position is set to true (blocked cell)
            for (int row = 0; row < boardSize; row++)
            {
                    
                for (int col = 0; col < boardSize; col++)
                {
                    
                    if ( rGen.NextDouble() <= cellMarkProb) 
                    {
                           
                    hiddenBoard[row,col] = true;
                    
                    //uses the position of the block to determine the value to add to the sum     
                    sumRow [row] += (col+1);
                    sumColumn [col] += (row+1);
                    
                    }
                
                }
                
            }
                
                
            // This is the main game-play loop.
            //---------------------------------------------------------------------------
            //
           
            bool gameNotQuit = true;
            while( gameNotQuit )
        
            {
                Console.Clear( );
                WriteLine( );
                WriteLine( "    Play Kakurasu!" );
                WriteLine( );
              
                
        
                    
            //Display Game Board        
            //-----------------------------------------------------------------------------          
                
                    for( int row = 0; row < boardSize; row ++ )
                    {
            
                    if( row == 0 )
                    {
                        Write( "        " );
                        for( int col = 0; col < boardSize; col ++ )
                        Write( $"  {letters[ col ]} "  );
                        WriteLine( );

                        Write( "        " );
                        for( int col = 0; col < boardSize; col ++ )
                        Write( " {0,2} ", col + 1 );
                        WriteLine( );
                        
                        Write( "        \u250c" );
                        for( int col = 0; col < boardSize - 1; col ++ )
                        Write( "\u2500\u2500\u2500\u252c" );
                        WriteLine( "\u2500\u2500\u2500\u2510" );
                    }

                    Write( "   {0} {1,2} \u2502", letters[ row ], row + 1 );

                    for( int col = 0; col < boardSize; col ++ )
                    {
                        if( userBoard[row,col] == true ) Write( " X \u2502" );
                        else   Write( "   \u2502" );
                    }
                            
                    // inserts a '0' in front of any int that is not a double digit
                    // this ensures uniform formatting of the game board 
                   
                    if (sumRow[row] < 10 && userSumRow[row] < 10) 
                    WriteLine( " 0{0} 0{1} ",sumRow[row], userSumRow[row]);
                            
                    else if (sumRow[row] < 10 && userSumRow[row]> 10) 
                    WriteLine( " 0{0} {1} ",sumRow[row],  userSumRow[row]);
            
                    else if (sumRow[row] > 10 && userSumRow[row]< 10) 
                    WriteLine( " {0} 0{1} ",sumRow[row],  userSumRow[row]);
                    
                    else  WriteLine( " {0} {1} ",sumRow[row],  userSumRow[row]);
    
        
                           
                    if( row < boardSize - 1 )
                    {
                    
                    Write( "        \u251c" );
                    for( int col = 0; col < boardSize - 1; col ++ )
                    Write( "\u2500\u2500\u2500\u253c" );
                    WriteLine( "\u2500\u2500\u2500\u2524" );
                    
                    }
                    else
                    {
                    Write( "        \u2514" );
                   
                    for( int col = 0; col < boardSize - 1; col ++ )
                    Write( "\u2500\u2500\u2500\u2534" );
                    WriteLine( "\u2500\u2500\u2500\u2518" );

                    Write( "         " );
                    
                    for( int col = 0; col < boardSize; col ++ )
                                
                    // if the column sum is a single digit a 0 is inserted in front of
                    // the digit to ensure uniform formatting 
            
                    if (sumColumn[col] < 10 ) Write( $" 0{sumColumn[col]} " );
                            
                    else Write( $" {sumColumn[col]} " );
                                 
        
                    WriteLine( );

                    Write( "         " );
                    
                    for( int col = 0; col < boardSize; col ++ )
                                
                    // the user column sum follows the same method of above for formatting 
                    
                    if (userSumColumn[col] < 10) Write( $" 0{userSumColumn[col]} " );
                                  
                    else Write( $" {userSumColumn[col]} " );
                    
                    WriteLine( );
                    }
                    
                    }// end for (int row = 0; row < boardSize; row ++ )
                   

                    // Gets the next move
                    //------------------------------------------------------------------------------

                    WriteLine( );
                    WriteLine( "   Toggle cells to match the row and column sums." );
                    Write(     "   Enter a row-column letter pair or 'quit': " );
                    string response = ReadLine( );

                    if( response == "quit" ) gameNotQuit = false;
                    else
                    {
                    
                    // checks that the user entered valid input (string with length of 2)
                    if (response.Length ==2) 
                    {
                         
                        string  rowLetter = response.Substring(0,1);
                        string columnLetter = response.Substring(1,1); 
                            
                        // searches the 1D letter array and returns the index of the first 
                        //occurance where the letter equals the rowLetter the user selected
                        int rowSelected = Array.IndexOf(letters, rowLetter);
                        int columnSelected = Array.IndexOf(letters, columnLetter);    
                            
                        // checks that the row and column selected are in range of the board
                        if (rowSelected >= 0 && rowSelected < boardSize && columnSelected >= 0 && columnSelected < boardSize)
                        {
                                
                            // if the user enters a box that they have previously entered it will unselect the box
                            if (userBoard [rowSelected, columnSelected] == true)
                            {
                                userBoard [rowSelected, columnSelected] = false;
                                userSumRow [rowSelected] -= columnSelected + 1;
                                userSumColumn[columnSelected] -= rowSelected + 1;
                                    
                            // if the box is currently unselected it will become selected 
                            } 
                            else
                            {
                                userBoard [rowSelected, columnSelected] = true;
                                  
                                // adds user sums in the columns and rows based
                                // on the position of the boxed marked 
                                userSumRow [rowSelected] += columnSelected + 1;
                                userSumColumn[columnSelected] += rowSelected + 1;
                                        
                            }
                                    
                            
                            }// ends check that the user input is in range
                        
                        }// ends for-loop
               
                 
                }
                
               }// end while (gameNotQuit)
               
            WriteLine( );
            
            
        }// end main method
    }// end class
}//end namespace


