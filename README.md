# python_samples
Samples of Python 3 applications on Mac, utilizing object-oriented development principles and line commenting. A text file is provided in the repository, but the contents are also displayed below:

#!/usr/bin/env python3
# Forrest Moulin
# Resume Helper is a python GUI application developed to facilitate the retrieval and copying of local resume files (txt, json) on Mac

from tkinter import *         # Import * from tkinter module
from tkinter import ttk       # Import ttk from tkinter module
import json                   # Import JSON module
from datetime import datetime # datetime is used to print the time that that the button was selected and the timestamp for file naming
import os                     # Import os to use terminal commands to move files to different directories
import getpass                # getpass is used to get username to facilitate creating and reading file names
import shutil                 # shutil module offers high-level file operations used to copy txt file contents

class ResumeHelper:           # Class definition
    
    def __init__(self, master): # Define __init__ method with self/master parameters           

        self.master = master # Used to manage tk window with other functions

        # File location object initialization
        self.user_name = getpass.getuser() # Get user name using getuser function from getpass module
        # Make mac user directory string that can be read using with open 
        self.mac_user_dir = '/Users/' + self.user_name 
        
        # Flat file name with .txt extension of read/copied backup file
        self.backup_text_file_name = 'Forrest_Moulin_Flat_Resume.txt' 
        # File directory used to locate backup text file
        self.backup_text_file_directory = self.mac_user_dir + '/BackupText/Directory/Path/' 
        # Designate backup text file location
        self.backup_text_file_pathname = self.backup_text_file_directory + self.backup_text_file_name 
        
        # Text file copy directory location
        self.text_file_copy_directory = self.mac_user_dir + '/TextCopy/Directory/Path/' 
            
        self.backup_json_file_name = 'Forrest_Moulin_Resume.json' # JSON file name of read/copied file
        # File directory used to locate backup json file
        self.backup_json_file_directory = self.mac_user_dir + '/BackupText/Directory/Path/' 
        # Designate json file location
        self.backup_json_file_pathname = self.backup_json_file_directory + self.backup_json_file_name 
        
        # JSON file copy directory location
        self.json_file_copy_directory = self.mac_user_dir + '/BackupJSON/Directory/Path/' 
        
        self.python_home_directory = self.mac_user_dir + '/Path/To/PythonHome/' # Python home directory location
        # Designate location of profile photo image
        self.profile_image_file_pathname = self.mac_user_dir + '/Path/To/Profile_Photo.png' 
 
        self.copy_text_count = int() # count variable implemented to help button listening without creating additional confirm button/logic
        self.copy_json_count = int() # count variable implemented to help button listening without creating additional confirm button/logic
                
        # First frame named header_frame_0
        self.header_frame_0 = ttk.Frame(master) # Create Frame using master as parent
        self.header_frame_0.pack(pady = 3) # Pack header_frame_0 into the top level window
        self.header_text_0 = ' Forrest Moulin '  # Used to assign text value to ttk Label 0 below
        
        # Overview Label (0)
        self.header_label_0 = ttk.Label(master, text = self.header_text_0, borderwidth = 1, relief = 'solid')
        self.header_label_0.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Profile label (1)
        self.header_label_1 = ttk.Label(master, text = ' 1. Profile ') # Create ttk label using master as parent
        self.header_label_1.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Academics label (2)
        self.header_label_2 = ttk.Label(master, text = ' 2. Academics ') # Create ttk label using master as parent
        self.header_label_2.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Awards label (3)
        self.header_label_3 = ttk.Label(master, text = ' 3. Awards ') # Create ttk label using master as parent
        self.header_label_3.pack(pady = 10) # Use geometry manager to pack label into the top level window
        
        # Professional Skills label (4)
        self.header_label_4 = ttk.Label(master, text = ' 4. Professional Skills ') # Create ttk label using master as parent
        self.header_label_4.pack(pady = 10) # Use geometry manager to pack the label into the top level window
        
        # Professional Experience label (5)
        self.header_label_5 = ttk.Label(master, text = ' 5. Professional Experience ') # Create ttk label using master as parent
        self.header_label_5.pack(pady = 10) # Use geometry manager to pack the label into the top level window
        
        # Create open file frame/buttons
        self.button_frame = ttk.Frame(master) # Frame that contains buttons
        self.button_frame.pack(pady = 10) # Use geometry manager to pack the button_frame into the top level window
        
        # Open Text File Button
        self.open_text_file_button = (
            ttk.Button(self.button_frame, text = 'Open Resume Text File', command = self.open_text_file_resume, width = 20))
        self.open_text_file_button.grid(row = 0, column = 0, padx = 5, pady = 5)
        
        # Open JSON File Buton
        self.open_json_file_button = (
            ttk.Button(self.button_frame, text = 'Open Resume JSON File', command = self.open_json_file_resume, width = 20))
        self.open_json_file_button.grid(row = 0, column = 1, padx = 5, pady = 5)
        
        # Copy Text File Button
        self.copy_text_file_resume_button = (
            ttk.Button(self.button_frame, text = 'Copy Text File Resume', command = self.copy_text_file_resume, width = 20))
        # grid function to place button is specific row-column pair
        self.copy_text_file_resume_button.grid(row = 1, column = 0, padx = 5, pady = 7) 
        
        # Copy JSON File Buton
        self.copy_json_file_resume_button = (
            ttk.Button(self.button_frame, text = 'Copy JSON File Resume', command = self.copy_json_file_resume, width = 20))
        # grid function to place button is specific row-column pair
        self.copy_json_file_resume_button.grid(row = 1, column = 1, padx = 5, pady = 7) 
        
        # Tk window profile image
        self.img = PhotoImage(file = self.profile_image_file_pathname) # Use PhotoIMage to load image onto Tk window
        Label(master,image = self.img).pack(pady = 10) # Pack the image into the Tk window
        
        # Message label/greeting
        self.message_greeting = StringVar() # String variable for message greeting text
        self.message_greeting = ' Welcome to Resume Helper! Please select a button to get started. '
        # Create ttk Label using master as parent
        self.message_label = ttk.Label(master, text = self.message_greeting, wraplength=700, justify=CENTER) 
        self.message_label.pack(pady = 10) # Pack message label into top level window
        
    def open_text_file_resume(self): # Define open text file function
        print('Open Resume text File button selected:') # Visibility of system status
        self.create_timestamp() # Timestamp metadata collection
        os.system('open -a TextEdit ' + self.backup_text_file_pathname) # Run terminal command to open the text file with Text Edit 
        
    def copy_text_file_resume(self): # Define copy text file function
        print('Copy Text File Resume button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata creation and collection   
        if self.copy_text_count != 1: # If the copy text count variable does not equal 1 (ready for copying)
            self.reset_listeners() # Clear all button listener variables (set both to 0)
            self.copy_text_count = 1 # Set copy text count variable to 1, allowing for file to be copied with same button on next click
            self.read_file(self.backup_text_file_pathname) # Read the backup text file
            print(self.data) # Print data read from the text file for debugging
            self.reduce_window_size(800, 800) # Reduce window size so console details are visibile on button click
            # Are you sure function to confirm that user would like to create a file copy 
            self.are_you_sure('text')
        else:
            text_file_copy_name = self.date_time_string + 'Forrest_Moulin_Resume.txt' # Assign a new text file name
            print('Creating ' + text_file_copy_name + '...') # Confirmation of new file name
            # Use shutil module to copy contents of file to a new file
            shutil.copyfile(self.backup_text_file_pathname, self.text_file_copy_directory + text_file_copy_name) 
            file_size = os.path.getsize(self.text_file_copy_directory + text_file_copy_name) # Document file size in bytes
            print('File size: '+str(file_size)+' bytes') # Print file size metadata to console
            label_text = 'Text file created:\n\'' + self.text_file_copy_directory + text_file_copy_name + '\'\n'
            print(label_text) # Visibility of system status in console
            self.message_label.config(text = label_text) # Update message label text
            self.reset_listeners() # Clear all button listener variables (set both to 0)
        
    def open_json_file_resume(self): # Define open json file function
        print('Open JSON File Resume button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/visibility of system status
        # Run terminal command to open the JSON file with TextEdit on Mac
        os.system('open -a TextEdit ' + self.backup_json_file_pathname) 
 
    def copy_json_file_resume(self): # Define copy json file function
        print('Copy JSON File Resume button selected:') # Visilbity of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        if self.copy_json_count != 1: # If the copy json count variable does not equal 1 (ready for copying)
#             self.reset_listeners() # Clear all button listener variables (set both to 0)
            self.copy_json_count = 1 # Set copy json count variable to 1, allowing for file to be copied with same button on next click
            self.read_file(self.backup_json_file_pathname) # Call read_file function using backup json file pathname as parameter
            print(json.dumps(self.data) + '\n') # Print data dictionary for debugging
            self.reduce_window_size(800, 800) # Reduce window size so console details are visibile on button click
            # Are you sure function to confirm that user would like to create a file copy 
            self.are_you_sure('json')
        else:
            json_file_copy_name = self.date_time_string + 'Forrest_Moulin_Resume.json' # Assign new JSON file name        
            print('Creating ' + json_file_copy_name + '...')  # Confirmation of new file name
            # Use shutil module to copy contents to a new file
            shutil.copyfile(self.backup_json_file_pathname, self.json_file_copy_directory + json_file_copy_name) 
            file_size = os.path.getsize(self.text_file_copy_directory + json_file_copy_name) # Document file size in bytes
            print('File size: '+str(file_size)+' bytes') # Print file size metadata to console        
            label_text = 'JSON file created\n' + self.json_file_copy_directory + json_file_copy_name + '\n'
            print(label_text) # Visibility of system status in console
            self.message_label.config(text = label_text) # Update message label text
#             self.reset_listeners() # Clear all button listener variables (set both to 0)
        self.reset_listeners() # Clear all button listeners (set both to 0)

    def create_timestamp (self):        # Define function to capture time and date (used in file naming)
        self.timestamp = datetime.now() # Capture timestamp using now function from datetime module
        # Create str that displays date using . separators in YYYY.MM.DD format
        self.date = str(self.timestamp.strftime('%Y.%m.%d')) 
        self.time = str(datetime.now().time())[:-7]     # time str used for timestamp on file name
        self.time = self.time.replace(':', '.')         # Replace timestamp colons with . for file name formatting
        self.date_time_string = self.date + '_' + self.time + '_' # Facilitate file naming
        print('date_time_string: ' + self.date_time_string)  # Print results for debugging
        print('Date: '+self.date+', Time: '+self.time+'\n')  
        
    def read_file(self, filepath): # Define function to read file based on pathname parameter
        self.filepath = filepath   # Define filepath variable using self
        with open(self.filepath, 'r') as file: # Read the text/json file according to the file path
            data = file.read() # Assign value of file contents to data dictionary variable
            self.data = data   # Declare self.data variable equal to data variable for use outside with open
        
    def set_geometry_string(self, width, height, left, top): # Define mutator function
        self.geometry_string = str(width) + 'x' + str(height) + '+' + str(left) + '+' + str(top)
        self.tk_window_width = width
        self.tk_window_height = height
        self.tk_window_left_position = left
        self.tk_window_top_position = top
        
    def get_geometry_string(self): # Define accessor function
        return self.geometry_string
        
    def set_max_screen_dimensions(self, width, height): # Define max screen dimensions mutator function
        self.max_screen_width = width
        self.max_screen_heigth = height
        
    def reduce_window_size(self, width, height): # Define reduce window size function used to manage tk window size
        self.tk_window_width = width # Reduce window width
        self.tk_window_height = height # Reduce window height
        # When file is copied, reduce window size so that details printed to the console are visible. Position in center of display
        self.set_geometry_string(self.tk_window_width, self.tk_window_height,
                                 int((self.max_screen_width-self.tk_window_width)/2), 0)
        self.master.geometry(self.geometry_string) # Update window geometry
    
    def set_tk_window_left_position(self, left): # Define tk window left position mutator function
        self.tk_window_left_position = int(left)
        
    def get_tk_window_left_position(self): # Define tk window left position accessor function
        return self.tk_window_left_position
        
    def set_tk_window_top_position(self, top): # Define tk window top position mutator function
        self.tk_window_top_position = int(top)
        
    def get_tk_window_top_position(self): # Define tk window top position accessor function
        return self.tk_window_top_position
    
    def set_tk_window_positions(self, left, top): # Define tk window positions mutator function
        self.tk_window_left_position = int(left)
        self.tk_window_top_position = int(top)
    
    def get_tk_window_width(self): # Define tk window width acessor function
        return self.tk_window_width
    
    def set_tk_window_width(self, width): # Define tk window width mutator function
        self.tk_window_width = int(width) # Return window width as integer
        
    def get_tk_window_height(self): # Define tk window width acessor function
        return self.tk_window_height
    
    def set_tk_window_height(self, height): # Define tk window width mutator function
        self.tk_window_height = int(height) # Return window height as integer
    
    def are_you_sure(self, filetype):
        pathname_var = str() # String object for pathname
        if filetype == 'json': # If file type is json, use json file path
            pathname_var = self.backup_json_file_pathname
            self.copy_json_count = 1 # Update count variable to allow file to be copied on next button click
        else: # Else if file type is text, use text file path
            pathname_var = self.backup_text_file_pathname
            self.copy_text_count = 1 # Update count variable to allow file to be copied on next button click
        label_text = ('Are you sure you would like to copy\n\'' + pathname_var +
            '\'?\nSelect the highlighted button a second time to create a copy of the ' + filetype + ' file.')
        self.message_label.config(text = label_text) # Update message label text
        
    def reset_listeners(self): # Define function to reset button listener variables to 0
        self.copy_json_count = 0
        self.copy_text_count = 0
    
def main():     # Define main method
    root = Tk() # Top level root window created with Tk constructor
    # Declare object resume_helper of class ResumeHelper and use root parameter to initialize the object    
    resume_helper = ResumeHelper(root) 
    print('Resume Helper initiated! :)') # Visibility of application startup
    resume_helper.create_timestamp() # Test create_timestamp function when main method is run and create date_time_string object
    root.title('Resume Helper!')  # Set root/tk window title
    
    # Format GUI window size to full screen width/height   
    resume_helper.set_tk_window_width(root.winfo_screenwidth()) # Define tk window width using the winfo display dimensions
    resume_helper.set_tk_window_height(root.winfo_screenheight()) # Define tk window height using the winfo display dimensions
    # Set screen width/height as max display size
    resume_helper.set_max_screen_dimensions(root.winfo_screenwidth(), root.winfo_screenheight()) 
    resume_helper.set_tk_window_positions(0, 0)
    # Concatenate geometry string in 'widthxheight+left_position+top_position' format
    resume_helper.set_geometry_string(resume_helper.get_tk_window_width(), resume_helper.get_tk_window_height(),
                                      resume_helper.get_tk_window_left_position(), resume_helper.get_tk_window_top_position())
    root.geometry(resume_helper.get_geometry_string()) # Use geometry string value as argument to format tk window size/position
    # Debugging and visibility of GUI metadata
    print('Window Left Position: '+str(resume_helper.get_tk_window_left_position()))
    print('Window Top Position: '+str(resume_helper.get_tk_window_top_position()))
    print('Display winfo width: '+str(resume_helper.get_tk_window_width()))
    print('Display winfo height: '+str(resume_helper.get_tk_window_height()))    
    root.mainloop()                # Used to loop the application in order to monitor for command events
    
if __name__ == '__main__': main()  # If __name__ variable equals '__main__', call the main method

# End of resume_helper.py

#!/usr/bin/env python3
# Python tkinter Country Feedback Form GUI Example with json, txt, sqlite3 implementations
# Developed by Forrest Moulin

from tkinter import *         # Import * from tkinter package
from tkinter import ttk       # Import ttk from tkinter module
import tkinter.font as font   # Font customization
import json                   # Import JSON module
import sqlite3                # Import sqlite3 module for interacting with databases
from datetime import datetime # datetime is used to print the time that that the buttons are selected/timestamp creation

class CountryForm:            # Class definition
    
    def __init__(self, master): # Define __init__ method with self and master parameters
        
        # Font configuration
        button_text_font = font.Font(family='Calibri', size=16)
        title_font = font.Font(family='Calibri', size=20)
        text_box_font = font.Font(family='Calibri', size=12)
        
        # First frame named frame_header
        self.frame_header = ttk.Frame(master)
        self.frame_header.pack(pady=5)  
        self.header_text = StringVar()
        self.header_text = ' Country Feedback Form - Version 1.0 ' 
        self.header_label = ttk.Label(self.frame_header, text=self.header_text, font=title_font, borderwidth=1, relief='solid')
        self.header_label.grid(row=0, column=0, padx=5, pady=5)
        # Combo box object created using root as parent, and country as the text variable
        self.combobox = ttk.Combobox(master, justify='center', state='readonly') 
        self.combobox.pack(pady=5) # Geometry manager pack method to pack combobox into top level window
        self.combobox.config(values=('U.S.', 'Canada', 'Mexico', 'Brazil', 'Portugal')) # Set string values for combo box option
        self.combobox.set('Select Country') # Set combobox pseudoprompt
        # When combobox is selected, use the callback method defined below
        self.combobox.bind('<<ComboboxSelected>>', self.combobox_callback) 
        self.country_string = str() # String variable that is updated based on combobox selection
        
        # Question 1
        self.question1 = ttk.Label(master, text = ' 1. What do you enjoy about this country? (Select all that apply) ',
                                   font=button_text_font)
        self.question1.pack(pady=5) # Use geometry manager to pack the first question into the top level window
        # Define string variables for question 1 answers
        self.selection1 = StringVar()
        self.selection2 = StringVar()
        self.selection3 = StringVar()
        self.selection4 = StringVar()
        self.selection5 = StringVar()
        self.question1_answer = StringVar()
        self.question1_string = str()
        # Create checkbutton objects
        self.checkbutton1 = ttk.Checkbutton(
            master, text='The people', variable=self.selection1, offvalue='', onvalue='people')
        self.checkbutton2 = ttk.Checkbutton(
            master, text='The landscape', variable=self.selection2, offvalue='', onvalue='landscape')
        self.checkbutton3 = ttk.Checkbutton(
            master, text='The climate', variable=self.selection3, offvalue='', onvalue='climate')
        self.checkbutton4 = ttk.Checkbutton(
            master, text='The entertainment', variable=self.selection4, offvalue='', onvalue='entertainment')
        self.checkbutton5 = ttk.Checkbutton(
            master, text='The opportunities', variable=self.selection5, offvalue='', onvalue='opportunities')

        # Use geometry manager to pack the check buttons
        self.checkbutton1.pack()
        
        self.checkbutton1
        self.checkbutton2.pack()
        self.checkbutton3.pack()
        self.checkbutton4.pack()
        self.checkbutton5.pack()

        # Create question 2, using master as parent) # Create question 2, using master as parent
        self.question2 = ttk.Label(master, text='2. What is your gender?', font=button_text_font)                             
        self.question2.pack(pady=10) # Use geometry manager to pack the second question into the top level window
        
        self.gender = StringVar() # Create gender string variable for question 2 answer
        self.gender.set('Gender_not_selected') # Set the gender variable to the default value of 'Gender not selected'
        self.question2_string = str() # String variable updates based on question 2 selection
        
        # Create radiobutton objects
        self.radiobutton1 = ttk.Radiobutton(master, text='Male', variable=self.gender, value='Male')
        self.radiobutton2 = ttk.Radiobutton(master, text='Female', variable=self.gender, value='Female')
        self.radiobutton3 = ttk.Radiobutton(master, text='Other', variable=self.gender, value='Other')
        self.radiobutton4 = ttk.Radiobutton(
            master, text='Prefer not to answer', variable=self.gender, value='Prefer not to answer')
        # Use geometry manager to pack the check buttons
        self.radiobutton1.pack()
        self.radiobutton2.pack()
        self.radiobutton3.pack()
        self.radiobutton4.pack()

        # Second frame named frame_content
        self.frame_content = ttk.Frame(master)
        self.frame_content.pack(pady=10) # Use geometry manager to pack the frame_content frame into the top level window
        # Question 3/Feedback
        ttk.Label(
            self.frame_content, text = '3. Please enter your feedback in the text box below:', font = button_text_font).grid(
                row=0, column=0, padx=5, pady=5)
        self.text_comments = Text(
            self.frame_content, font = text_box_font, width = 50, height = 10, highlightbackground = 'black',
            highlightcolor = 'black', highlightthickness=1, wrap=WORD) # Text box formatting
        self.text_comments.grid(row = 1, column=0, padx=10, pady=10) # Load text box into frame
        self.question3_answer = StringVar() # String var for interacting with text box
        self.question3_string = str() # String for printing/file creation
        
        self.button_frame1 = ttk.Frame(master) # Frame that contains submit and clear buttons
        self.button_frame1.pack(pady=5) # Use geometry manager to pack the button_frame1 into the top level window
        
        self.print_button = ttk.Button(self.button_frame1, text = 'Print Form', command = self.print_form) # Create print form button
        self.print_button.grid(row = 0, column=0, padx=5, pady=5) # Use .grid() method to place submit button in button_frame1
        self.clear_button = ttk.Button(self.button_frame1, text = 'Clear Form', command = self.clear_form) # Create clear form button
        self.clear_button.grid(row = 0, column=1, padx=5, pady=5) # Use .grid() method to place clear button in button_frame1
        
        self.file_frame = ttk.Frame(master) # Frame that contains file buttons
        self.file_frame.pack(pady=10) # Use geometry manager to pack the file_frame into the top level window
        
        # Create file dump buttons        
        self.flat_button = ttk.Button(self.file_frame, text = 'Flat File', command = self.flat_file, width = 12)
        self.flat_button.grid(row = 0, column=0, padx=5, pady=5)
        self.json_button = ttk.Button(self.file_frame, text = 'JSON File', command = self.json_file, width = 12)
        self.json_button.grid(row = 0, column=1, padx=5, pady=5)
        # SQLite Button
        self.sqlite_button = ttk.Button(self.file_frame, text = 'SQLite Database', command = self.sqlite, width = 12)
        self.sqlite_button.grid(row = 0, column=2, padx=5, pady=5)
 
    def combobox_callback(self, event): # Define callback method using self and event parameters
        # Update header_label depending on country selection
        self.header_label.config(text = ' ' + self.combobox.get() + ' Feedback Form - Version 1.0 ') 
        print(self.combobox.get(), 'selected\n') # Print country selection for debugging
        self.combobox.selection_clear()       # Clear the highlighted selection in the combobox
    
    def clear_checkboxes(self):      # Define method to clear checkboxes
        print('-Checkboxes cleared (Question 1):', end = ' ') # Print message that shows checkboxs are being cleared
        print(self.question1_string)                         # Print selected checkboxes as string before clearing
        self.selection1.set('')                              # Checkboxes set to offvalues
        self.selection2.set('')
        self.selection3.set('')
        self.selection4.set('')
        self.selection5.set('')
    
    def update_strings(self): # Define update_strings method to get/update appropriate strings for form priting/file creation
        if self.combobox.get() == 'Select Country':     # If combobox is not selected
            self.country_string = 'No_country_selected' # Set country string to default value
        else:                                              # Else print as intended
            self.country_string = str(self.combobox.get()) # Country string set to value of combobox selection
        # Concatenate question 1 answers
        self.question1_answers = (self.selection1.get() + ' ' + self.selection2.get() + ' ' +
                                 self.selection3.get() + ' ' + self.selection4.get() + ' ' + self.selection5.get()) 
        # Strip white space to the right and left of the str
        self.question1_string = re.sub(' +', ' ', str(self.question1_answers.strip())) 
        
        if len(self.question1_string) == 0: # If the string length is 0
            self.question1_string = 'No_answers_selected' # Set the question1 answer to the default null value
        # Question 2 string set to value of gender variable
        self.question2_string = str(self.gender.get())
        # Question 3 string set to the value of the question 3 answer text box
        # Get the text box from line 1, character 0 to the end of the text box
        self.question3_answer = self.text_comments.get(1.0, 'end') 
        self.question3_string = str(self.question3_answer.strip()) # Strip the string to remove whitespace/new line at the end
        if len(self.question3_string) == 0:                        # If the string length is 0
            self.question3_string = 'No_feedback_provided'         # Set question 3 string to default no feedback provided value        
        
    def print_form(self):    # Define print form method (to sonsole)   
        print('Print button selected:\n' + self.format_title_bar()) # Print for debugging
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.update_strings() # get string variables from form selection
        print('-Country:', self.country_string) # Print country selection
        print('-Question 1 selection(s)/answer string:', self.question1_string) # Print Question 1 selection(s)         
        print('-Question 2 selection/answer string: ' + self.question2_string)  # Print the gender selection
        print('-Question 3 feedback/answer string:', self.question3_string) # Print the feedback from the text box
        print(self.format_title_bar() + '\nForm printed\n') # Formatting
        
    def clear_form(self): # Define clear method
        print('Clear button selected:\n'+ self.format_title_bar()) # Visibility of clear form function
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.update_strings() # get string variables from form selection
        print('-Country cleared from the drop down selection:', self.country_string) # Print country selection
        
        self.header_label.config(text=self.header_text) # Clear the header label
        self.combobox.set('Select Country')               # Set combobox value to display 'Select Country' as a pseudoprompt
        self.combobox.selection_clear()                   # Clear the highlighted selection in the combobox      
        self.clear_checkboxes()                           # Clear check boxes by setting the variables to offvalue of ''
        
        # Clear the radio button selections by setting the gender variable back to its default value of 'Gender not selected'
        print('-Radio button cleared (Question 2):', self.gender.get()) # Print the question 2 value being cleared
        self.gender.set('Gender_not_selected') # Reset gender variable to default value (updates question 2 radio button selections)
        
        if len(self.question3_string) > 0:                   # If the text box string is not null,
            self.text_comments.delete(1.0, 'end')            # Delete text field from line 1, char 0 to the end
        print('-Feedback cleared (Question 3): ' + self.question3_string) # Print the question 3 value being cleared
        print(self.format_title_bar() + '\nForm cleared\n') # Visibility of system status
   
    # Input/Output documentation - https://docs.python.org/3/tutorial/inputoutput.html
    def flat_file(self):                    # Define flat file method
        print('Flat file button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.update_strings()                # Update all strings before creating file
        # Concatenate the string to be used for flat file
        self.flat_string = (self.country_string + '\n' + self.question1_string + '\n' +
                            self.question2_string + '\n' + self.question3_string) 
        self.flat_file_string = self.date_time_string + 'Forrest_Moulin_Country_Form.txt' # Flat file name
        print('Creating ' + self.flat_file_string + '...') # Visibility of function status
        self.output = open(self.flat_file_string, 'w') # Write output using flat_file_string as .txt file name
        for line in self.flat_string: # For each line in the flat string
            self.output.write(line)   # Write a new line to the flat file output
        self.output.close()           # Close output
        print('Flat file created\n')  # Visibility of function completion
   
    # JSON documentation: https://www.json.org/json-en.html
    # JSON decoder/encoder: https://docs.python.org/3.5/library/json.html#json.JSONEncoder
    def json_file(self):
        print('JSON button selected:') # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.update_strings()           # Update all strings before creating file
        # Convert python strings to JSON strings and print for debugging
        print(json.dumps(self.country_string))  
        print(json.dumps(self.question1_string))
        print(json.dumps(self.question2_string))
        print(json.dumps(self.question3_string))
        
        self.data = { # Create dictionary to store strings for form answers
            'Country' : self.country_string,
            'Question 1' : self.question1_string,
            'Question 2' : self.question2_string,
            'Question 3' : self.question3_string
        }
        print(json.dumps(self.data), end = '\n\n') # Debugging
        self.date_time_string = str()
        self.json_file_string = self.date_time_string + 'Forrest_Moulin_Country_Form.json' # JSON file name
        print('Creating ' + self.json_file_string + '...') # Debugging
        with open(self.json_file_string, 'w') as outfile: # Print to JSON file
            json.dump(self.data, outfile)                 # Dump data dictionary to json outfile
        print('JSON file created\n')                      # Visiblity of system status
    
    # sqlite3 documentation: https://docs.python.org/3.5/library/sqlite3.html
    def sqlite(self):                    # Define sqlite method to create in-memory database   
        print('SQLite button selected:') # Visibility of system status
        self.create_timestamp()          # Debugging/timestamp metadata collection
        self.update_strings()             # Update all strings before creating file
        # db variable set to value of sqlite connect method with
        # 'file_name.db' as the ':memory:' name (in place of file name)
        db = sqlite3.connect('forrest_moulin_sqlite_file.db')
        cur = db.cursor() # cur variable set to value of db cursor
        cur.execute('drop table if exists test') # Drop (delete) table if test table already exists
        # Create a new table named test that stores string and int column values
        cur.execute(''' # Table of name test with columns/parameters (number, string)
            create table test( 
            number INT,
            string TEXT
            )''')
        # Insert python string variables and assign integer to each. ? is used to assign variable parameters
        cur.execute('insert into test(number, string) values (?, ?)', (0, self.country_string))
        cur.execute('insert into test(number, string) values (?, ?)', (1, self.question1_string))
        cur.execute('insert into test(number, string) values (?, ?)', (2, self.question2_string))
        cur.execute('insert into test(number, string) values (?, ?)', (3, self.question3_string))        
        for row in cur.execute('select * from test'): # For each row in the table, select all data from the row and print it
            print(row)
        db.close() # Close the database connection
        print('')  # Formatting for next output
        
    def create_timestamp (self):        # Define function to capture time and date (used in file naming)
        self.timestamp = datetime.now() # Capture timestamp using now function from datetime module
        self.date = str(self.timestamp.strftime('%m.%d.%Y')) # Create str that displays date using . separators in MM.DD.YYYY format
        self.time = str(datetime.now().time())[:-7]     # time str used for timestamp on file name
        self.time = self.time.replace(':', '.')         # Replace timestamp colons with . for file name formatting
        self.date_time_string = self.date + '_' + self.time + '_' # Facilitate file naming
        print('Date: '+self.date+', Time: '+self.time+'\n') # Visibility of date and time

    def format_title_bar(self): # Define function to create title bar with - characters (length 100)
        self.title_format_bar = ''
        for x in range(100):
            self.title_format_bar += '-'
        return self.title_format_bar
    
def main():                         # Define main method  
    root = Tk()                     # Top level root window created with Tk constructor
    countryform = CountryForm(root) # Declare object countryform of class CountryForm
    display_width = root.winfo_screenwidth() # Get display resolution width
    display_height = root.winfo_screenheight() # Get display resolution height
    root.title('Country Form - Forrest Moulin') # Tk instance title (window title)
    root.geometry(str(display_width)+'x'+str(display_height)) # Set root window to display resolution size (maximized)
    root.mainloop()                  # Used to loop the application in order to monitor for command events
    print(dir(CountryForm))
if __name__ == '__main__': main()    # If __name__ variable equals '__main__', call the main method

# End of country_form.py

#!/usr/bin/env python3
# Traffic Light utilizes GPIO Zero modules as well as sleep, pause, and datetime to provide visibility/audibility
# of a simple traffic light sequence
# Developed by Forrest Moulin

# gpiozero documentation: https://gpiozero.readthedocs.io/en/stable/faq.html
from gpiozero import LED, Button, Buzzer 
 # Import sleep to use time increments to limit input and make scheduled changes to the LED and button variables
from time import sleep  
from signal import pause # Pause allows application to wait until a signal like when_pressed or when_released
from datetime import datetime # datetime is used to print the time that that the button was pressed/released
  
class TrafficLight:
    def __init__(self):
        # Button and buzzer objects
        self.button = Button(2) # Assign GPIO/BCM pin 2 (SDA on T_Extension) to the button variable
        self.buzzer = Buzzer(3) # Assign GPIO/BCM pin 3 (SCL on T_Extension) to the buzzer variable
        self.buzzer.off() # Turn buzzer off (or else...BEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEP)
        # Traffic light led objects/GPIO pin assignments
        self.greenled = LED(26) # Assign GPIO/BCM pin 26 (B26 on T_Extension) to the greenled variable
        self.yellowled = LED(19) # Assign GPIO/BCM pin 19 (B19 on T_Extension) to the yellowled variable
        self.redled = LED(13) # Assign GPIO/BCM pin 17 (B13 on T_Extension) to the redled variable
        self.redled.on() # Turn red light on when application is run
        # Time duration variables
        self.max_green_time = 4 # Set maximum time for green light illumination
        self.max_yellow_time = 2 # Set maximum time for yellow light illumination
        self.set_time_increments() # Time variable instantiation

    def button_pressed(self): # Define button pressed method used to assign when_pressed callback
        print('Button pressed at: ' + self.get_time())
        
    def button_released(self): # Define button_released method
        print('Button released at :' + self.get_time() + '\n')
        self.set_time_increments() # Time variables reset on button release

        self.go_green() # Green light method
            
        if self.green_time == 0: # Once green light time is exhausted,
            self.go_yellow() # call yellow light method

        if self.yellow_time == 0: # Once yellow time is exhausted,
            self.go_red() # call red light method
        
    def get_time(self): # Define get time method to return timestamp used to display button and light statuses
        t = str(datetime.now().time())[:-7] # Create str that displays time released in long time format
        return t # Return the time stamp string
        
    def set_time_increments(self):
        # Time variables
        self.green_time = self.max_green_time # Time that the green light will be on (in seconds)
        self. yellow_time = self.max_yellow_time # Time that the yellow light will be on (in seconds)
        self.buzzer_increment = 0.5 # Increment of time in seconds that the light will flash and the buzzer will sound
        
    def go_green(self): # Green light method
        if self.green_time == self.max_green_time: # Visibility of green light status
            print('Green light on at: ' + self.get_time()) 

        while self.green_time >= self.buzzer_increment: # While the green light still has time left,
            self.redled.off() # Turn off the redled once the green led is on
            self.buzzer.on() # Turn the buzzer on
            self.greenled.on() # Turn on the green led
            sleep(self.buzzer_increment) # Wait designated increment with the buzzer on
            self.green_time -= self.buzzer_increment # Reduce the amount of green time according to sleep time
            self.buzzer.off() # Turn the buzzer off
            sleep(self.buzzer_increment) # Wait designated increment with the buzzer off
            self.green_time -= self.buzzer_increment # Reduce the amount of green time according to sleep time
              
    def go_yellow(self): # Yellow light method
        self.greenled.off() # Turn the green led off
        print('Green timed out at: ' + self.get_time()) # Print for debugging
        self.yellowled.on() # Turn on yellow led
        print('Yellow light on at: ' + self.get_time()) # Visibility of yellow light status
        sleep(self.max_yellow_time) # Wait while yellow led is on
        self.yellow_time -= self.max_yellow_time # Reduce yellow time to 0
        self.yellowled.off() # Turn yellow led off

    def go_red(self): # Red light method
        print('Yellow timed out at: ' + self.get_time()) # Visibility of yellow light status
        self.redled.on() # Turn red led on
        print('Red light on at: ' + self.get_time()) # Visibility of red light status

def main(): # Define main method
    trafficlight = TrafficLight() # Class instance object
    print('Traffic Light application started at: ' + trafficlight.get_time() + '\n') # Print timestamp
    trafficlight.button.when_released = trafficlight.button_released # Assign button released callback method
    trafficlight.button.when_pressed = trafficlight.button_pressed # Assign button pressed callback method
    pause() # Wait for button signals
    
if __name__ == "__main__": # If __name__ variable equals "__main__", call the main method
    try:
        main()
    except KeyboardInterrupt: # Allow keyboard interrupt (Ctrl + C)
        print('Keyboard Interrupt with \'Ctrl\' + \'C\'')
        exit(0) # Turn off LEDs, Buzzers, Buttons, and exit application

# End of traffic_light.py

#!/usr/bin/env python3
# SSHcripts Galore
# Developed by Forrest Moulin
# scripts_galore.py uses tkinter modules to create a GUI application to create SSH client connections, run scripts from a Raspberry Pi
# This Python file utilizes a Zsh script on Mac and other files on a Windows computer
# Certin lines are commented out to allow this example to run succcessfully without file resources

import paramiko                # Import paramiko library
from tkinter import *          # Import * from tkinter module
from tkinter import ttk        # Import ttk from tkinter module
import json                    # Import JSON module
# from time import sleep       # Import sleep to use time increments 
import os                      # Import os to use terminal commands to move files to different directories
from datetime import datetime  # datetime is used to print the time that that the button was selected and the timestamp for file naming
# Buzzer module used for audibility of system status on Raspberry Pi https://gpiozero.readthedocs.io/en/stable/                  
# from gpiozero import Buzzer

class PythonProject: # Class definition
    
    def __init__(self, master, host, user, psw): # Define __init__ method with self, master, other parameters       
        self.host = host                         # Declare self.host variable
        self.user = user                         # Declare self.user variable
        self.key  = '/path/to/rsa_public_key'    # Path to RSA public key file (located on Mac)
        self.psw = psw                           # Declare self.psw variable
#         # Declare variable ssh and use SSHClient method from paramiko library to create a SSH client object
#         self.ssh = paramiko.SSHClient()
#         # Declare variable sshW for Windows machine and use SSHClient method from paramiko library to create a SSH client object
#         self.sshW = paramiko.SSHClient()
#         # Use the set_missing_host_key_policy method since no host keys are being used
#         self.ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
#         # Establish an SSH connection with the connect() method
#         self.ssh.connect(host,username = user, key_filename = self.key, password = psw) 
        
        # First frame named frame_header
        self.frame_header = ttk.Frame(master)
        self.frame_header.pack(pady=10)
        # set header text/formatting
        self.header_text = StringVar()
        self.header_text = "Overview"
        self.header_label = ttk.Label(self.frame_header, text = self.header_text, borderwidth=1, relief = "solid")
        self.header_label.pack(pady=10)
        self.header_label.grid(row=0, column=0, padx=5)
         
        # 'GUI' section
        self.header1 = ttk.Label(master, text = "1: Python GUI (You're looking at it)")
        self.header1.pack(pady=10)
            
        # Mac Attack section
        self.header2 = ttk.Label(master, text = "2: Mac Attack") # Create section Label, using master as parent
        self.header2.pack(pady=10) # Use geometry manager to pack the second question into the top level window
        
        # IoPi section
        self.header3 = ttk.Label(master, text = "3. IoPi") # Create section Label, using master as parent
        self.header3.pack(pady=10)
        
        # Windows section
        self.header4 = ttk.Label(master, text = "4: Windows to the West") # Create section Label, using master as parent
        self.header4.pack(pady=10) # Use geometry manager to pack the label into the top level window into the top level window            
        
        # File sharing section
        self.header5 = ttk.Label(master, text = "5: Care to Share?") # Create section Label, using master as parent
        self.header5.pack(pady=10) # Use geometry manager to pack the label into the top level window     
        
        self.mac_button_frame = ttk.Frame(master) # Frame that contains submit and clear buttons
        self.mac_button_frame.pack(pady=10) # Use geometry manager to pack the mac_button_frame into the top level window
        # Create Start Mac Script button
        self.start_mac_script_button = ttk.Button(
            self.mac_button_frame, text = 'Start Mac Script', command=self.start_mac_script, width=20) 
        self.start_mac_script_button.grid(row=0, column=0, padx=5) # Use .grid() method to place submit button in mac_button_frame
        # Create stop Mac Script button
        self.stop_mac_script_button = ttk.Button(
            self.mac_button_frame, text = 'Stop Mac Script', command=self.stop_mac_script, width=20) 
        self.stop_mac_script_button.grid(row=0, column=1, padx=5) # Use .grid() method to place clear button in mac_button_frame
        
        # Windows SSH Connection Buttons
        self.windows_button_frame = ttk.Frame(master) # Frame that contains Windows buttons
        self.windows_button_frame.pack(pady=10)
        # Windows open connection button
        self.windows_connection_button = ttk.Button(
            self.windows_button_frame, text = "Windows Connection", command=self.open_windows_connection, width=20)
        self.windows_connection_button.grid(row=0, column=0, padx=5)
        # Windows close connection button
        self.windows_close_button = ttk.Button(
            self.windows_button_frame, text = "Close Windows Connection", command=self.close_windows_connection, width=20)
        self.windows_close_button.grid(row=0, column=1, padx=5)
        
        # Create file dump frame
        self.file_frame = ttk.Frame(master) # Frame that contains file buttons
        self.file_frame.pack(pady=10) # Use geometry manager to pack the file_frame into the top level window
        # Flat file button
        self.flat_button = ttk.Button(self.file_frame, text = "Flat File", command=self.flat_file, width=20)
        self.flat_button.grid(row=0, column=0, padx=5)
        # JSON fil button
        self.json_button = ttk.Button(self.file_frame, text = "JSON File", command=self.json_file, width=20)
        self.json_button.grid(row=0, column=1, padx=5)
        
#         self.img=PhotoImage(file='/path/to/Profile_Photo.png') # Use PhotoImage to load image onto Tk window
#         Label(master,image=self.img).pack(pady=10)
        
    def start_mac_script(self):                   # Define start Scripts method
        print("Mac script opened:") # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        # Send terminal command via paramiko ssh client exec_command
        # Use osacript command to set volume and second command to confirm the change using the say
        stdin,stdout,stderr = self.ssh.exec_command(
            'osascript -e "set Volume 3.5" &; say "Volume set to 3.5 out of 7 by using the oh sa script Terminal command to run AppleScript code"') 
#         sleep(7) # Wait 7 seconds before executing next command
#         # Send a command to confirm it was received on the MacBook
#         stdin,stdout,stderr = self.ssh.exec_command( 
#             'say "You are now connected to Forrest\'s MacBook Pro via SSH. Well done chap!"') 
        sleep(5) # Wait 5 seconds before executing next command
        # scrip_file_name.zsh executes various commands on a MacBook Pro        
        stdin,stdout,stderr = self.ssh.exec_command('zsh script_file_name.zsh') 
        
    def stop_mac_script(self):                 # Define stop_mac_script method
        print("Mac script stopped:")    # Visibility of System Status
        self.create_timestamp() # Debugging/timestamp metadata collection
#         self.ssh.exec_command('killall zsh')   # Send SSH command to killall zsh scripts being executed on the Mac
#         sleep(1)                               # Wait 1 second for first command to execute
#         self.ssh.exec_command('killall Music') # Send SSH command to killall Music applications (turn off music if playing)

    def open_windows_connection(self):
        print("Windows connection opened:") # Visiblity of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
#         self.sshW.set_missing_host_key_policy(paramiko.AutoAddPolicy()) # Use the set_missing_host_key_policy method since no host keys are being used        
#         # Update variable values for Windows Connection
#         self.host = 'WindowsIPAddress'
#         self.sshW.connect(self.host, username = self.user, key_filename = self.key, password = self.psw)
#         self.print_results() # Print results to show new connection details in hidden format
#         # PowerShell command exceuted view paramiko ssh client
        stdin,stdout,stderr = self.sshW.exec_command(
            'PowerShell -Command "Add-Type –AssemblyName System.Speech; (New-Object System.Speech.Synthesis.SpeechSynthesizer).Speak(\'Hello!\');"') 
        
    def close_windows_connection(self):
        print("Windows connection closed:") # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
#         self.ssh.close() # Close ssh client connection
    
    def flat_file(self):                     # Define flat file method
        print("Flat file button selected:")  # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.flat_string = "Title: SSHcript Galore" + "\n" +\
        "Feature 1: Create a Python GUI application providing options for SSH connection to multiple network devices." +\
        "\n" + "Feature 2: Use SSH to run Mac Automator applications, zsh scripts, and AppleScript scripts over the network." +\
        "\n" + "Feature 3: Use SSH to access three IoT devices and accomplish specific tasks (either directly or indirectly "\
        "using a Mac/Windows/iPhone device)." + "\n" +\
        "Feature 4: Use SSH to access a Windows machine, get network configuration values, run a PowerShell script, "\
        "and execute a VBA script." + "\n" +\
        "Feature 5: Create a file server that allows network devices to read and write to files on a shared network folder."\
        "Additionally, mount a USB drive and windowshare drive automatically on startup."
        self.flat_file_string = self.date_time_string + "Forrest_Moulin_Flat_File.txt" # Flat file name
        print("Creating " + self.flat_file_string + "...") # Debugging
        self.output = open(self.flat_file_string, 'w') # Write output using flat_file_string as .txt file name
        for line in self.flat_string:                  # For each line in the flat string
            self.output.write(line)                    # Write a new line to the flat file output
        self.output.close()                            # Close output
        print("Flat file created!\n")                  # Visibility of system status
        print("Flat file string: \n" + self.flat_string + "\n") # Print the flat string for debugging/visibility
        # Move the flat file to appropriate directory in the share folder
#         os.system('mv /path/to/directory/' + self.flat_file_string + ' /share') 
   
    # JSON info: https://www.json.org/json-en.html
    # JSON decoder/encoder: https://docs.python.org/3.5/library/json.html#json.JSONEncoder
    def json_file(self):
        print("JSON button selected:") # Visibility of system status
        self.create_timestamp() # Debugging/timestamp metadata collection
        self.data = {           # Create data dictionary to store strings in JSON format
            'Title' : 'SSHcripts Galore',
            'Feature 1: Python GUI' : 'Create a Python GUI application providing options for SSH connection to multiple network devices.',
            'Feature 2: Mac Attack' : 'Use SSH to run Mac Automator applications, zsh scripts, and AppleScript scripts over the network.',
            'Feature 3: IoPi' : 'Use SSH to access three IoT devices and accomplish specific tasks (either directly or indirectly using '\
            'a Mac/Windows/iPhone device).',
            'Feature 4: Windows to the West' : 'Use SSH to access a Windows machine, get network configuration values, run '\
            'a PowerShell script, and execute a VBA script.',
            'Feature 5: Care to Share?' : 'Create a file server that allows network devices to read and write to files on '\
            'a shared network folder. Additionally, mount a USB drive and windowshare drive automatically on startup.'
        }
        self.json_file_string = self.date_time_string + "Forrest_Moulin_JSON.json" # JSON file name
        print("Creating " + self.json_file_string + "...") # Debugging
        with open(self.json_file_string, 'w') as outfile:  # Print to JSON file
            json.dump(self.data, outfile)                  # Dump data dictionary to json outfile
        print("JSON file created!\n")                      # Visiblity of system status
        print(json.dumps(self.data) + '\n')                # Debugging
        # Move the json file to appropriate directory in the share folder
#         os.system('mv /path/to/directory/' + self.json_file_string + ' /new_directory') 
  
    def print_results(self): # Define print_results method, which will print hidden hostname, username, and password to the console. 
        print("SSH Connection Results") # Visibility of system status
        hidden_host = ""
        hidden_user = ""
        hidden_psw = ""
        
        # Loop to convert host string characters to * characters, leaving .'s and last 3 characters visible
        for x in range(len(self.host)):  
            if(x < len(self.host) -3):   # If the current index x is less than the host length minus 3 characters,
                if(self.host[x] == "."): # And if the character is a ., don't convert it to an *
                    hidden_host += "."
                else:                    # Else convert the character to an *
                    hidden_host += "*"
            else:                        # Always show the last 3 characters of host
                hidden_host += self.host[x] 
            
        for y in range(len(self.user)): # Loop to convert user string characters to * characters, leaving the last 3 visible
            if(y < len(self.user) - 3): # If the current index y is less than the username length minus 3 characters,
                hidden_user += "*"      # convert the character to *
            else:                       # Else show the last 3 characters of user
                hidden_user += self.user[y]              
                
        for z in range(len(self.psw)): # Loop to convert user password characters to * characters
            hidden_psw += "*"
        
        # Print hidden variables for visibility of system status 
        print("Host:", hidden_host + "\nUsername:", hidden_user + "\n" + "Password:", hidden_psw + "\n") 
        
    def create_timestamp (self):        # Define function to capture time and date (used in file naming)
        self.timestamp = datetime.now() # Capture timestamp using now function from datetime module
        self.date = str(self.timestamp.strftime('%m.%d.%Y')) # Create str that displays date using . separators in MM.DD.YYYY format
        self.time = str(datetime.now().time())[:-7]     # time str used for timestamp on file name
        self.time = self.time.replace(':', '.')         # Replace timestamp colons with . for file name formatting
        self.date_time_string = self.date + '_' + self.time + '_' # Facilitate file naming
        print('date_time_string: ' + self.date_time_string)  # Print results for debugging
        print('Date: '+self.date+', Time: '+self.time+'\n')          
    
    def buzzer_function(self):
        buzzer = Buzzer(3) # Assign GPIO/BCM pin 3 (SCL on T_Extension) to the buzzer variable
        # Use buzzer to indicate that application is running - audibility of system status
        buzzer.on() # Buzz for 0.25 seconds
        sleep(0.25)
        buzzer.off() # Silence for 0.25 seconds
        sleep(0.25)
        buzzer.on() # Buzz for 0.25 seconds
        sleep(0.25)
        buzzer.off() # Silence for 0.25 seconds
        sleep(0.25)
        buzzer.on() # Buzz for 0.25 seconds
        sleep(0.25)
        buzzer.off() # Indefinite silence
   
def main(): # Define main method  
    root = Tk() # Top level root window created with Tk constructor
    # Declare object pythonproject of class PythonProject and use parameters to initialize the object
    pythonproject = PythonProject(root, 'MacIPAddress', 'MacUsername', 'RSAPassword')  
    pythonproject.print_results() # Print the connection results for visibility of system status
#     pythonproject.buzzer_function() # Audibility of system application startup (Raspberry Pi only)
    root.title("SSHcript Galore - Forrest Moulin") # Set root/tk window title
    root.geometry("800x800+320+0")   # Initialize window with 800 width and 800 height. Left position 320, 
    root.mainloop()                  # Used to loop the application in order to monitor for command events
    
if __name__ == "__main__": main()    # If __name__ variable equals "__main__", call the main method

# End of sshcripts_galore.py

# Zsh script example utilized in sscripts_galore application
#!/bin/zsh
sleep 3 # Wait 3 seconds before next command
# Music time!
open IST402_Final_Mac.zsh # Open this script for viewing
open /Users/forrestmoulin/Library/Speech/Speakable\ Items/Don\'t_Stop_Me.app
# PSU browser launch example / background
open https://www.psu.edu/
# Crescendo simulation
osascript -e 'tell application "Music" to set the sound volume to 50'
osascript -e 'set volume output volume 10' -e 'delay 3' -e 'set volume output volume 15' -e 'delay 3' -e 'set volume output volume 20' -e 'delay 3'
osascript -e 'set volume output volume 25' -e 'delay 3' -e 'set volume output volume 30' -e 'delay 3' -e 'set volume output volume 35' -e 'delay 3'
osascript -e 'set volume output volume 40' -e 'delay 3' -e 'set volume output volume 45' -e 'delay 3' -e 'set volume output volume 50' -e 'delay 3'
osascript -e 'set volume output volume 55' -e 'delay 2' -e 'set volume output volume 60' -e 'delay 2' -e 'set volume output volume 65' -e 'delay 2'
osascript -e 'set volume output volume 70' -e 'delay 2' -e 'set volume output volume 75' -e 'delay 2' -e 'set volume output volume 80'
#Decrescendo simulation
osascript -e 'delay 3' -e 'set volume output volume 70' -e 'delay 3' -e 'set volume output volume 60 -e' -e 'delay 3' -e 'set volume output volume 50'
sleep 1.5
#open /Users/forrestmoulin/Library/Speech/Speakable\ Items/Dust.app
osascript -e 'tell application "Music" to play track "Another One Bites The Dust"'
osascript -e 'tell application "Music" to set the sound volume to 40' -e 'delay 3' -e 'tell application "Music" to set the sound volume to 30' -e 'delay 3'
osascript -e 'tell application "Music" to set the sound volume to 25'
# Serena time!
say "Welcome aboard our trip through Forrest's home network. My name is Serena, and I will be your personal guide today."; sleep 0.25 # Pause for 0.25 seconds
osascript -e 'tell application "Music" to set the sound volume to 20'
say "Please buckle up, and keep all limbs close by as we begin our journey at the speed of light!"
osascript -e 'tell application "Music" to set the sound volume to 15'
say "Of course, I am no A I, but I am as smart as they come! Well, at least that's what my master says."; sleep 0.25 # Pause for 0.25 seconds
osascript -e 'tell application "Music" to set the sound volume to 10'
say "To address the Project's Second requirement of using Secure Shell to run Mac Automator applications, zee shell scripts, and AppleScript code, let's start by changing a few settings on the MacBook Pro. Shall we?"; sleep 1 # Wait 1 second
osascript -e 'tell application "Music" to set the sound volume to 5'
osascript -e 'quit app "Firefox"' # Close Firefox app with Applescript
osascript -e 'quit app "Music"' # Close Music app with AppleScript
say "The network setup shell command can be used to turn MacBook Wi-Fi off without super user do permissions. We can run this command on this very zee shell script."
networksetup -setairportpower en0 off; sleep 1 # Wait 1 second
say "Notice how the Wi-Fi icon on the Macbook indicates that Wi-Fi has been turned off."; sleep 1 # Wait 1 second
say "Likewise, we can use the network setup shell command to turn MacBook Wi-Fi back on without super user do permissions."
networksetup -setairportpower en0 on; sleep 1 # Wait 1 second
say "The MacBook Wi-Fi has been turned back on. Let's see if we can address the project's third requirement and get some light in here."; sleep 1 # Wait 1 second
open https://developers.google.com/
say "Hey Google"; sleep 0.25 # Wait 1/4 second
say "Turn on desk lamp"; sleep 6 # Wait 6 seconds
say "Cheers! That's much better!"; sleep 1 # Wait 1 second
say "Hey Google"; sleep 0.25 # Wait 1/4 second
say "How's the weather today?"; sleep 12 # Wait 12 seconds
say "Good to know! I'd say we can turn off the fan now."; sleep 0.25 # Wait 0.25 seconds
say "Hey Google"; sleep 0.25 # Wait 0.25 seconds
say "Turn off the fan"; sleep 6 # Wait 6 seconds
say "Brilliant!"; sleep 0.25 say "How about we try to give my other friend a ring, eh?"; sleep 0.5 # Wait 0.5 seconds
osascript -e 'quit app "Firefox"' # Use AppleScript to close Firefox
# For Alexa to hear the voice without recognizing that it is not a human voice, it is best to set the MacBook volume at 45% or lower (as needed)
open https://developer.amazon.com/en-US/alexa
say "Alexa"; sleep 0.5; say "What is today's date?"; sleep 5
say "Thanks for that"; sleep 0.5; say "Alexa"; sleep 0.5; say "Set a reminder"; sleep 4; say "Remind me to turn in my Raspberry Pi project"; sleep 4; say "In thirty minutes"; sleep 8
say "Great! Let's carry on"; sleep 0.5 # Wait 0.5 seconds
say "We just used a Raspberry Pi to connect to a MacBook pro, which spoke aloud, triggering Google Home Mini and Amazon Echo Dot device commands to accomplish real-world tasks such as turning on a desk lamp, turning off a fan, and setting a reminder."
sleep 0.5 # Wait 0.5 seconds
osascript -e 'quit app "Firefox"' # Use AppleScript to close Firefox
say "We have also demonstrated how a Python application running on a Raspberry Pi can use paramiko's secure shell client to connect to a Mac device on the same network."
sleep 1 # Wait 1 second
say "Such an architecture allows the Raspberry Pi to make configuration changes, run scripts and applications, and power off a Mac device via a shared network connection. As a result, a user can access their device via ethernet or Wi-Fi connections without physically accessing the device or its user interface."
sleep 0.5 # Pause for 0.5 seconds
say "While there are other ways of invoking shell interfaces in order to send multiple commands, my master found that the most efficient way of accessing all of the MacBook's resources is to create a zee shell script on the Mac that is executed with the exec command method from paramiko."
sleep 0.5 # Pause for 0.5 seconds
open https://developer.apple.com/
say "My master loves AppleScript, so let's run a few files. Shall we?"; sleep 0.5 # Pause for 0.5 seconds
say "3"; sleep 0.25 # Pause for 0.25 seconds
say "2"; sleep 0.25 # Pause for 0.25 seconds
say "1"; sleep 0.25 # Pause for 0.25 seconds
say "Action!"; sleep 0.25 # Pause for 0.25 seconds
osascript -e 'set volume output volume 45' -e 'say "The MacBook output volume has been set to 45% via AppleScript"' -e 'delay 0.5' -e 'say "And we were able to run multiple AppleScript commands on one line by placing the e option in front of each command as well as placing each command inside single quotation marks."' -e 'delay 0.5' -e 'say "AppleScript can be useful since its commands do not require super user do permissions, and commands can be run from the Terminal or another shell such as the invisible shell created through this SSH connection."' -e 'delay 0.5'
say "Lastly, I will execute an Automator application by using the open command followed by the pathname of the Lock_Screen file."; sleep 0.5 # Wait 0.5 seconds
open /Users/forrestmoulin/Library/Speech/Speakable\ Items/Lock_Screen.app; sleep 3 # Wait for 3 three seconds
say "We have now demonstrated Requirements 1 through 3 of the project. Unfortunately, my work here is complete. I will now defer to my colleague on the northern side of the coast, who honestly is not as cool as I am... LOL!"
osascript -e 'quit app "Firefox"' # Use AppleScript to close Firefox
open https://developer.microsoft.com/en-us/
# End of Zsh script
