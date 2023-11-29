from tkinter import Button,Label
import random 
import mysettings
import ctypes
import sys

class Cell:
    all = []
    cell_count = mysettings.CELL_COUNT
    cell_count_label_object= None
    def __init__(self,x ,y, is_mine=False):
        self.is_mine = is_mine
        self.is_opened= False
        self.is_mine_candidate= False
        self.cell_but_object = None 
        self.x = x
        self.y = y
        
        Cell.all.append(self)
        
    def create_btn_object(self, location): #instance method
            
            btn = Button(
                
                location,
                width=12,
                height=4,
                # text = f"{self.x}, {self.y}"
            )
            #left click -3 right click
            btn.bind('<Button-1>', self.left_click_actions)
            btn.bind('<Button-3>', self.right_click_actions)
            self.cell_but_object = btn
    @staticmethod        
    def create_cell_count_label(location):
        lbl = Label(
            location,
            bg='black',
            fg='white',
            
            text = f"cells left: {Cell.cell_count}",
            width=12,
            height=4,
            font=("",30)
        )
        Cell.cell_count_label_object = lbl
                
            
    def left_click_actions(self, event):
        # print(event)
        # print("i am left clicked")
        if self.is_mine: # if self.is_mine = True
            self.show_mine()
        else:
            if self.surrounded_cells_mines_length ==0:
                for cell_obj in self.surrounded_cells:
                    cell_obj.show_cell()
            self.show_cell()
            #if mines count is equal to cell count so i won the game
            if Cell.cell_count == mysettings.MINES_COUNT:
                ctypes.windll.user32.MessageBoxW(0, 'YOU WON THE GANE','game over', 0)
                
        #cancel all the left and right click events if the cell is already opened
        self.cell_but_object.unbind('<Button-1>')    
        self.cell_but_object.unbind('<Button-3>')    
    
    def get_cell_by_axis(self,x,y):
         #return a cell object based on values of x and y
        for cell in Cell.all:
            if cell.x == x and cell.y == y :
                return cell   
    @property         
    def surrounded_cells(self):
        cells = [
            self.get_cell_by_axis(self.x-1, self.y-1),
            self.get_cell_by_axis(self.x-1, self.y),
            self.get_cell_by_axis(self.x-1, self.y+1),
            self.get_cell_by_axis(self.x, self.y-1),
            self.get_cell_by_axis(self.x+1, self.y-1),
            self.get_cell_by_axis(self.x+1, self.y),
            self.get_cell_by_axis(self.x+1, self.y+1),
            self.get_cell_by_axis(self.x, self.y+1)
        ]
        
        cells= [cell for cell in cells if cell is not None]  
        return cells   
    @property
    def surrounded_cells_mines_length(self):
        counter = 0
        for cell in self.surrounded_cells:
            if cell.is_mine:
                counter +=1
        return counter        
       
    def show_cell(self):
        # print(self.get_cell_by_axis(0,0))
        if not self.is_opened:
            Cell.cell_count -=1

            # print(self.surrounded_cells_mines_length)
            self.cell_but_object.configure(text= self.surrounded_cells_mines_length)
            
            #replace the text of the cell count label to the newer count
            if Cell.cell_count_label_object:
                Cell.cell_count_label_object.configure(
                    text= f"cells left: {Cell.cell_count}"
                )
            #if this is a mine so we will return the color to SystemButtonFace    
            self.cell_but_object.configure(
                bg='SystemButtonFace'
            )
        #mark the cell as opened         
        self.is_opened = True    
    
    def show_mine(self):
        #logic to end the game and that the pleayer lost
        self.cell_but_object.configure(bg='red')
        ctypes.windll.user32.MessageBoxW(0, 'you clicked on a mine','game over', 0)
        sys.exit()
        
        
        
    def right_click_actions(self, event):
        if not self.is_mine_candidate:
            self.cell_but_object.configure(
                bg='orange'
            )
            self.is_mine_candidate=True
        else:
            self.cell_but_object.configure(
                bg='SystemButtonFace'
            )
            self.is_mine_candidate= False
    
        
    @staticmethod
    def randomize_mines():
            # my_list=['mohamned', 'omar', 'wafaa']
            # my_picked = random.sample(my_list, 2)
            # print(my_picked)
            picked_cells = random.sample(Cell.all, mysettings.MINES_COUNT)
            # print(picked_cells)
            for pick_cell in picked_cells:
                pick_cell.is_mine = True
        
    def __repr__(self): #magic method-remember
        return f"cell({self.x}, {self.y})"        
                
