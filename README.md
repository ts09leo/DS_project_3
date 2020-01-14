#include <iostream>
#include <stdlib.h>
#include <time.h>
#include "../include/algorithm.h"

using namespace std;

/******************************************************
 * In your algorithm, you can just use the the funcitons
 * listed by TA to get the board information.(functions
 * 1. ~ 4. are listed in next block)
 *
 * The STL library functions is not allowed to use.
******************************************************/

/*************************************************************************
 * 1. int board.get_orbs_num(int row_index, int col_index)
 * 2. int board.get_capacity(int row_index, int col_index)
 * 3. char board.get_cell_color(int row_index, int col_index)
 * 4. void board.print_current_board(int row_index, int col_index, int round)
 *
 * 1. The function that return the number of orbs in cell(row, col)
 * 2. The function that return the orb capacity of the cell(row, col)
 * 3. The function that return the color fo the cell(row, col)
 * 4. The function that print out the current board statement
*************************************************************************/


struct block{
    int priority;
    int pos_x;
    int pos_y;
};

void algorithm_A(Board board, Player player, int index[]){
    char color = player.get_color();
    block B[5][6];

    for(int i = 0; i < 5; i++){
        for(int j = 0; j < 6; j++){
            B[i][j].priority = 0;
            if(color == board.get_cell_color(i, j) || board.get_cell_color(i, j) == 'w'){}
            else{
                B[i][j].priority = -1;
            }
            B[i][j].pos_x = i;
            B[i][j].pos_y = j;
        }
    }

    for(int i = 0; i < 5; i++){
        for(int j = 0; j < 6; j++){
            if(color == board.get_cell_color(i, j) || board.get_cell_color(i, j) == 'w'){
                    B[i][j].priority = 5-board.get_capacity(i, j)+board.get_orbs_num(i, j);
                    if(i-1 >= 0 && B[i][j].priority > 0){
                        if(board.get_cell_color(i-1, j) != color && board.get_cell_color(i-1, j) != 'w'){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i-1, j)-board.get_orbs_num(i-1, j)){
                                B[i][j].priority += 10;
                            }
                            if(board.get_capacity(i-1, j)-board.get_orbs_num(i-1, j) == 1){
                                B[i][j].priority = 0;
                            }
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j)==1){
                                B[i][j].priority += 100;
                            }
                        }
                        else if(board.get_cell_color(i-1, j) == color){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i-1, j)-board.get_orbs_num(i-1, j)){
                                B[i][j].priority += 3;
                            }
                        }
                    }
                    if(i+1 < 5 && B[i][j].priority > 0){
                        if(board.get_cell_color(i+1, j) != color && board.get_cell_color(i+1, j) != 'w'){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i+1, j)-board.get_orbs_num(i+1, j)){
                                B[i][j].priority += 10;
                            }
                            if(board.get_capacity(i+1, j)-board.get_orbs_num(i+1, j) == 1){
                                B[i][j].priority = 0;
                            }
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j)==1){
                                B[i][j].priority += 100;
                            }
                        }
                        else if(board.get_cell_color(i+1, j) == color){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i+1, j)-board.get_orbs_num(i+1, j)){
                                B[i][j].priority += 3;
                            }
                        }
                    }
                    if(j-1 >= 0 && B[i][j].priority > 0){
                        if(board.get_cell_color(i, j-1) != color && board.get_cell_color(i, j-1) != 'w'){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i, j-1)-board.get_orbs_num(i, j-1)){
                                B[i][j].priority += 10;
                            }
                            if(board.get_capacity(i, j-1)-board.get_orbs_num(i, j-1) == 1){
                                B[i][j].priority = 0;
                            }
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j)==1){
                                B[i][j].priority += 100;
                            }
                        }
                        else if(board.get_cell_color(i, j-1) == color){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i, j-1)-board.get_orbs_num(i, j-1)){
                                B[i][j].priority += 3;
                            }
                        }
                    }
                    if(j+1 < 6 && B[i][j].priority > 0){
                        if(board.get_cell_color(i, j+1) != color && board.get_cell_color(i, j+1) != 'w'){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i, j+1)-board.get_orbs_num(i, j+1)){
                                B[i][j].priority += 10;
                            }
                            if(board.get_capacity(i, j+1)-board.get_orbs_num(i, j+1) == 1){
                                B[i][j].priority = 0;
                            }
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j)==1){
                                B[i][j].priority += 100;
                            }
                        }
                        else if(board.get_cell_color(i, j+1) == color){
                            if(board.get_capacity(i, j)-board.get_orbs_num(i, j) <= board.get_capacity(i, j+1)-board.get_orbs_num(i, j+1)){
                                B[i][j].priority += 3;
                            }
                        }
                    }
            }
            else{
                B[i][j].priority = -1;
            }
        }
    }

    for(int i = 0; i < 5; i ++){
        for(int j = 0; j < 6; j++){
            cout<<B[i][j].priority<<" ";
        }
        cout<<endl;
    }
    int top = 0;
    int same = 1;
    for(int i = 0; i < 5; i++){
        for(int j = 0; j < 6; j++){
            if(B[i][j].priority > top){
                top = B[i][j].priority;
                same = 1;
                cout<<"top = "<<top<<endl;
            }
            else if(B[i][j].priority == top){
                same += 1;
                cout<<"i = "<<i<<" j = "<<j<<endl;
            }
        }
    }
    cout<<"same = "<<same<<endl;
    block Max[same];
    int k = 0;
    for(int i = 0; i < 5; i++){
        for(int j = 0; j < 6; j++){
            if(B[i][j].priority == top){
                Max[k] = B[i][j];
                k+=1;
            }
        }
    }
    for(int i = 0; i < same; i++){
        cout<<"("<<Max[i].pos_x<<" "<<Max[i].pos_y<<")"<<endl;
    }
    block Final;
    srand(time(NULL)*time(NULL));
    int Rand = rand()%same;
    Final = Max[Rand];
    index[0] = Final.pos_x;
    index[1] = Final.pos_y;
    return;
}
