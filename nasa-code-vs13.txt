#Gneral aim: The motif finder program can do more than just finding motifs. Thus the name will be changed to "Sequence Analyzer".
#In this file I will provide a more sophisticated GUI in which the old code will be embedded.

#The project is completed for the moment and will be tested for applications (04.05.2023)

import tkinter as tk
from tkinter import ttk
from PIL import Image, ImageTk

import re

import pyperclip as pc
#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~#

# Classes used for GUI

class Main_win_Seq_An():
    def __init__(self, seq_an_root):
        self.seq_an_root = seq_an_root
        self.seq_an_title = sequence_analyzer_root.title("NASA - Nucleic Acid Sequence Analyzer - Version 1.13")
        self.seq_an_geometry = sequence_analyzer_root.geometry("1400x700")
        

class Menu_win_Seq_an(Main_win_Seq_An):
    def __init__(self):
        self.menu_frame_seqa = tk.Frame(sequence_analyzer_root, 
                                        bg="#e0dcdc", 
                                        highlightbackground="black", 
                                        highlightthickness=1)
        self.menu_frame_seqa.pack(side=tk.LEFT)
        self.menu_frame_seqa.propagate(False) # prevents manual size changes of frame
        self.menu_frame_seqa.configure(width=250, height=700)

        self.func_frame_seqa = tk.Frame(sequence_analyzer_root, 
                                        bg="#c4c0c0", 
                                        highlightbackground="grey", 
                                        highlightthickness=1)
        self.func_frame_seqa.pack(side=tk.LEFT)
        self.func_frame_seqa.propagate(False)
        self.func_frame_seqa.configure(width=1150, height=700)

        self.developed_by = tk.Label(self.func_frame_seqa,
                                     text="developed by Attila Placido Sachslehner",
                                     bg="#c4c0c0",
                                     font=("arial", 12))
        self.developed_by.place(x=850, y=670)


        ##~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~BUTTONS & their Indicator~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~##

        self.button_style = ttk.Style()
        self.button_style.configure("Custom.TButton", font=("arial", 15))

        self.home_button = ttk.Button(self.menu_frame_seqa, 
                                      text="Home", 
                                      style="Custom.TButton", 
                                      command=lambda: show_index(self.home_index, frame_change=change_to_home_frame()))
        self.home_button.place(x=22, y=100) #Note: place instead of pack() in order to place widgets more accurately


        #Make indication to show current button/menupoint
        self.home_index = tk.Label(self.menu_frame_seqa, 
                                   text="", 
                                   background="#e0dcdc")
        self.home_index.place(x=10, y=99, width=5, height=35)


        self.intro_button = ttk.Button(self.menu_frame_seqa, 
                                       text="Introduction", 
                                       style="Custom.TButton", 
                                       command=lambda: show_index(self.intro_index, frame_change=change_to_intro_frame()))
        self.intro_button.place(x=22, y=200)

        self.intro_index = tk.Label(self.menu_frame_seqa, 
                                    text="", 
                                    background="#e0dcdc")
        self.intro_index.place(x=10, y=199, width=5, height=35)


        self.seq_analyzer_button = ttk.Button(self.menu_frame_seqa, 
                                              text="Analyze Sequence", 
                                              style="Custom.TButton", 
                                              command=lambda: show_index(self.seq_analyzer_index, frame_change=change_to_seq_an_frame()))
        self.seq_analyzer_button.place(x=22, y=300)

        self.seq_analyzer_index = tk.Label(self.menu_frame_seqa, 
                                           text="", 
                                           background="#e0dcdc")
        self.seq_analyzer_index.place(x=10, y=299, width=5, height=35)


        self.contact_button = ttk.Button(self.menu_frame_seqa, 
                                         text="Contact", 
                                         style="Custom.TButton", 
                                         command=lambda: show_index(self.contact_index, frame_change=change_to_contact_frame()))
        self.contact_button.place(x=22, y=400)

        self.contact_index = tk.Label(self.menu_frame_seqa, 
                                      text="", 
                                      background="#e0dcdc")
        self.contact_index.place(x=10, y=399, width=5, height=35)

        self.exit_button = ttk.Button(self.menu_frame_seqa, 
                                      text="Close", 
                                      style="Custom.TButton", 
                                      command=lambda: exit_program())
        self.exit_button.place(x=22, y=500)

        
    ##~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~Frame_DEFINITIONS~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~##

        #home frame
        self.home_window_frame = tk.Frame(self.func_frame_seqa, width=1150, height=700, bg="#c4c0c0")

        self.home_window_frame.rowconfigure(0, weight=1)
        self.home_window_frame.columnconfigure((0,1,2), weight=1)

        ##~~~~~~~~~~~~~~~~~~~~~~~Home-Frame-labels~~~~~~~~~~~~~~~~~~~~##

        self.home_window_text = tk.Label(self.home_window_frame, 
                                            text="Welcome to NASA\n(Nucleic Acid Sequence Analyzer)",
                                            bg="#c4c0c0", 
                                            font=("arial", 20))
        self.home_window_text.grid(row=0, column=1)

        self.nasa_image_open = Image.open("NASA_Logo.tif").resize((800, 225))
        self.nasa_image_convert = ImageTk.PhotoImage(self.nasa_image_open)
        self.nasa_image_use = ttk.Label(self.home_window_frame, image=self.nasa_image_convert)
        self.nasa_image_use.grid(row=1, column=1, pady=20)
        
        
        self.home_information = tk.Label(self.home_window_frame,
                                         text="NASA provides a flexible search function\nfor specific patterns such as\ntranscription factor binding sites,\nrepeats, and more in nucleic acids.",
                                         bg="#c4c0c0",
                                         font=("arial", 20))
        self.home_information.grid(row=2, column=1)
        
        ##~~~~~~~~~~~~~~~~~~~~~~~Home-Frame-labels~~~~~~~~~~~~~~~~~~~~##
        

        #intro frame
        self.intro_window_frame = tk.Frame(self.func_frame_seqa, width=1150, height=700, bg="#c4c0c0")

        self.intro_window_frame.rowconfigure(0, weight=1)
        self.intro_window_frame.columnconfigure((0,1,2), weight=1)

        ##~~~~~~~~~~~~~~~~~~~~~~~Intro-Frame-labels~~~~~~~~~~~~~~~~~~~~##

        self.intro_window_text = tk.Label(self.intro_window_frame, 
                                              text="Introduction/brief manual",
                                              bg="#c4c0c0", 
                                              font=("arial", 20))
        self.intro_window_text.grid(row=0, column=1)


        ##~~~~~~~~~~~~NASA Intro 1: Pattern input~~~~~~~~~~##
        self.intro_nasapattern_load = Image.open("nasa_pattern.png").resize((400, 102))
        self.intro_nasapattern_convert = ImageTk.PhotoImage(self.intro_nasapattern_load)
        self.intro_nasapattern_use = tk.Label(self.intro_window_frame, image=self.intro_nasapattern_convert, bg="grey")
        self.intro_nasapattern_use.grid(row=1, column=0, pady=15)

        self.intro_nasapattern_text = tk.Label(self.intro_window_frame, 
                                                text="Provide a pattern in the corresponding text box.\nFor a flexible search write the different nucleotides\nin brackets and sepparate them with |.\nThis example will find\nTTAAT and TTTAT.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasapattern_text.grid(row=1, column=1, pady=15, padx=10, sticky=tk.NSEW)

        self.intro_nasapattern_error = tk.Label(self.intro_window_frame, 
                                                text="Possible errors:\n1) Avoid white spaces eg. after copy-paste.\nThey could lead to no result even\nif there should be a correct target.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasapattern_error.grid(row=1, column=2, pady=15)


        ##~~~~~~~~~~~~NASA Intro 2: Promoter input~~~~~~~~~~##

        self.intro_nasapromoter_load = Image.open("nasa_promoter.png").resize((400, 125))
        self.intro_nasapromoter_convert = ImageTk.PhotoImage(self.intro_nasapromoter_load)
        self.intro_nasapromoter_use = tk.Label(self.intro_window_frame, image=self.intro_nasapromoter_convert, bg="grey")
        self.intro_nasapromoter_use.grid(row = 2, column = 0, pady=15)

        self.intro_nasapromoter_text = tk.Label(self.intro_window_frame, 
                                                text="1) Provide a nucleic acid sequence in fasta\nformat (e.g. >ATGGTAAC). Skip the name of the\nsequence as only acgtuACGTU\nare allowed charakters.\n2) Press the Find Pattern button\nto start the search.\n3) The clear & new search button will\nreset the search window.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasapromoter_text.grid(row=2, column=1, pady=15, padx=10, sticky=tk.NSEW)

        self.intro_nasapromoter_error = tk.Label(self.intro_window_frame, 
                                                text="Possible errors:\n1) Avoid white spaces eg. after copy-paste.\nThey could lead to no result even\nif there should be a correct target.\n2) Other charakter than ACGTU provided.\n3) Sequence not in fasta format.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasapromoter_error.grid(row=2, column=2, pady=15)


        ##~~~~~~~~~~~~NASA Intro 3: Search results~~~~~~~~~~##
        
        self.intro_nasaresult_load = Image.open("nasa_result.png").resize((400, 111))
        self.intro_nasaresult_convert = ImageTk.PhotoImage(self.intro_nasaresult_load)
        self.intro_nasaresult_use = tk.Label(self.intro_window_frame, image=self.intro_nasaresult_convert, bg="grey")
        self.intro_nasaresult_use.grid(row = 3, column = 0, pady=15)

        self.intro_nasaresult_text = tk.Label(self.intro_window_frame, 
                                                text="The search results will show up in a small\nwindow. One can copy individual lines or use\nthe copy results button to copy all\nresults to the clipboard.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasaresult_text.grid(row=3, column=1, pady=15, padx=10, sticky=tk.NSEW)

        self.intro_nasaresult_error = tk.Label(self.intro_window_frame, 
                                                text="Possible errors:\nNo errors reported yet.\nPlease report new errors.\nThe contact can be found in\nthe Contact section.", 
                                                bg="#c4c0c0",
                                                font=("arial", 12))
        self.intro_nasaresult_error.grid(row=3, column=2, pady=15)

        ##~~~~~~~~~~~~~~~~~~~~~~~Intro-Frame-labels~~~~~~~~~~~~~~~~~~~~##
        
        
        #seq an frame
        self.seq_analyzer_window_frame = tk.Frame(self.func_frame_seqa, width=1150, height=700, bg="#c4c0c0")

        self.seq_analyzer_window_frame.rowconfigure(0, weight=1)
        self.seq_analyzer_window_frame.columnconfigure((0,1), weight=1)

        
        #seq an frame scrollbar
        self.seq_analyzer_canvas = tk.Canvas(self.seq_analyzer_window_frame, width=1100, height=700, background="#c4c0c0", highlightbackground="#c4c0c0", highlightthickness=0)
        self.seq_analyzer_canvas.grid(row=0, column=1, sticky="nesw", padx=15)

        self.seq_analyzer_scrollbar = ttk.Scrollbar(self.seq_analyzer_window_frame, orient="vertical", command=self.seq_analyzer_canvas.yview)
        self.seq_analyzer_scrollbar.grid(row=0, column=2, sticky="ns")

        self.seq_analyzer_window_scrol = tk.Frame(self.seq_analyzer_canvas, width=1125, height=700, bg="#c4c0c0")
        self.seq_analyzer_window_scrol.bind("<Configure>", lambda e: self.seq_analyzer_canvas.configure(scrollregion=self.seq_analyzer_canvas.bbox("all")))

        self.seq_analyzer_canvas.create_window((0,0), window=self.seq_analyzer_window_scrol, anchor="nw")
        self.seq_analyzer_canvas.configure(yscrollcommand=self.seq_analyzer_scrollbar.set)
        #seq an frame scrollbar
        

        
        ##~~~~~~~~~~~~~~~~~~~~~~~Seqan-Frame-labels~~~~~~~~~~~~~~~~~~~~##

        self.seq_analyzer_window_text = tk.Label(self.seq_analyzer_window_scrol, 
                                                     text="Analyze your sequence of interest",
                                                     bg="#c4c0c0", 
                                                     font=("arial", 20))
        self.seq_analyzer_window_text.grid(row=0, column=1)

        ##~~~~~~~~~Seqan-functions~~~~~~~~##

        ##-1-## motif find function

        dna_check_var = "^>[ACGTUagctu]+$"

        def pattern_find_function(): #for find_motif button
            tf_BS = self.enter_pattern.get().upper() #Entry widget
            dna_in = self.promoter_entry.get(1.0, tk.END).upper() #Text widget

                
            if len(self.enter_pattern.get()) == 0 and self.promoter_entry.compare("end - 1c", "==", "1.0"):
                self.mot_pro_miss_variable = tk.Label(self.result_frame, textvariable=self.mot_pro_miss, bg="#c4c0c0", font=("arial", 12))
                self.mot_pro_miss_variable.grid(column=1)

            elif len(self.enter_pattern.get()) == 0:
                self.motif_miss_variable = tk.Label(self.result_frame, textvariable=self.motif_miss, bg="#c4c0c0", font=("arial", 12)) #check wether motif textbox is empty (Entry widget)
                self.motif_miss_variable.grid(column=1)
                      

            elif self.promoter_entry.compare("end - 1c", "==", "1.0"):
                self.promoter_miss_variable = tk.Label(self.result_frame, textvariable=self.promoter_miss, bg="#c4c0c0", font=("arial", 12)) #check wether promoter textbox is empty (Text widget)
                self.promoter_miss_variable.grid(column=1)

            else:
                if re.search(pattern=dna_check_var, string=dna_in) == None: #checks for right input format (i.e. nucleic acid sequence in fastaformat)
                    self.dna_in_check_variable = tk.Label(self.result_frame, textvariable=self.dna_in_check, bg="#c4c0c0", font=("arial", 12))
                    self.dna_in_check_variable.grid(column=1)
                    

                else:
                    self.general_info_variable = tk.Label(self.result_frame, textvariable=self.general_info, bg="#c4c0c0", font=("arial", 12))
                    self.general_info_variable.grid(column=1)
                 
                    if re.search(pattern=tf_BS, string=dna_in) is None:
                        self.first_re_search = tk.Label(self.result_frame, text="No pattern found", compound="top", bg="#c4c0c0", foreground="red", font=("arial", 12))
                        self.first_re_search.grid(column=1)

                    else:

                        for motif in re.finditer(pattern=tf_BS, string=dna_in):
                            self.search_results = tk.Text(self.result_frame, height=1, borderwidth=0, font=("arial", 12))
                            self.search_results.insert(1.0, motif)
                            self.search_results.grid(column=1)
                            self.search_results.configure(state="disabled")

                            self.find_pattern.configure(state=tk.DISABLED)
      

        ##-2-## delete motif textbox function

        def clear_new_search():
            for widgets in self.result_frame.winfo_children():
                widgets.destroy()

            self.find_pattern.configure(state=tk.NORMAL)
            
        
        ##-3-## delete motif textbox function

        def clear_text_pattern():
            self.enter_pattern.delete(0, tk.END)

        
        ##-4-## delete promoter textbox function

        def clear_text_prom():
            self.promoter_entry.delete(1.0, tk.END)

        
        ##-5-## right click menu function

        def re_click_menu(seq_analyzer_window_frame):
            global right_click
            right_click = tk.Menu(self.seq_analyzer_window_frame, tearoff=0)
            right_click.add_command(label="Copy", accelerator="Ctrl+C")
            right_click.add_command(label="Cut", accelerator="Ctrl+X")
            right_click.add_command(label="Paste", accelerator="Ctrl+V")
            right_click.add_separator()
            right_click.add_command(label="select all", accelerator="Ctrl+A")


        ##-6-## right click menu function for text widgets

        def sel_all_Txt(event=None):
            self.promoter_entry.tag_add(tk.SEL, "1.0", tk.END)
            self.promoter_entry.mark_set(tk.INSERT, "1.0")
            self.promoter_entry.see(tk.INSERT)
            return "break"


        ##-7-## function for showing right click menu

        def show_re_menu(event):
            widget = event.widget
            right_click.entryconfigure("Copy",command=lambda:widget.event_generate("<<Copy>>"))
            right_click.entryconfigure("Cut",command=lambda:widget.event_generate("<<Cut>>"))
            right_click.entryconfigure("Paste",command=lambda:widget.event_generate("<<Paste>>"))
    
            if type(widget) == tk.Entry:
                right_click.entryconfigure("select all",command=lambda:widget.select_range(0, tk.END)) 
        
            elif type(widget) == tk.Text:
                right_click.entryconfigure("select all",command=sel_all_Txt)
    
            right_click.tk.call("tk_popup", right_click, event.x_root, event.y_root)

        re_click_menu(self.seq_analyzer_window_frame)

        self.seq_analyzer_window_frame.bind_class("Text", "<Button-3><ButtonRelease-3>", show_re_menu)
        self.seq_analyzer_window_frame.bind_class("Entry", "<Button-3><ButtonRelease-3>", show_re_menu)


        ##-8-## function for copy all results

        def copy_results(): #copy all
            
            self.copy_results_text = []

            for results in self.result_frame.winfo_children():
                if isinstance(results, tk.Text):
                    self.copy_results_text.append(results.get(1.0, tk.END))


            pc.copy(str(self.copy_results_text))
              

        ##~~~~~~~~~Seqan-GUI-elements~~~~~~~~##

        self.pattern_request = tk.Label(self.seq_analyzer_window_scrol, text="Insert your pattern in the box below:", bg="#c4c0c0", font=("arial", 14))
        self.pattern_request.grid(row=1, column=1, pady=5)

        self.enter_pattern = tk.Entry(self.seq_analyzer_window_scrol, width=40, justify=tk.CENTER, font=("arial", 12))
        self.enter_pattern.grid(row=2, column=1, pady=5)

        self.del_entry_pattern = tk.Button(self.seq_analyzer_window_scrol, text="Clear Pattern Textbox", font=("arial", 12), command=clear_text_pattern)
        self.del_entry_pattern.grid(row=3, column=1, pady=5)

        self.promoter_request = tk.Label(self.seq_analyzer_window_scrol, text="Insert your promoter sequence in the box below:", bg="#c4c0c0", font=("arial", 14))
        self.promoter_request.grid(row=4, column=1, pady=5)

        self.promoter_entry = tk.Text(self.seq_analyzer_window_scrol, width=109, height=12, font=("courier new", 12))
        self.promoter_entry.grid(row=5, column=1, pady=5)

        self.del_promoter_txt = tk.Button(self.seq_analyzer_window_scrol, text="Clear Promoter Textbox", font=("arial", 12), command=clear_text_prom)
        self.del_promoter_txt.grid(row=6, column=1, pady=5)

        self.seqan_button_frame = tk.Frame(self.seq_analyzer_window_scrol, bg="#c4c0c0")
        self.seqan_button_frame.grid(row=7, column=1)

        self.seqan_button_frame.rowconfigure(0, weight=1)
        self.seqan_button_frame.columnconfigure((0,1,2), weight=1)

        self.find_pattern = tk.Button(self.seqan_button_frame, text="Find Pattern", font=("arial", 12), fg="green", command=pattern_find_function)
        self.find_pattern.grid(row=0, column=0, pady=5, padx=5)

        self.clear_screen_btn = tk.Button(self.seqan_button_frame, text="clear & new search", font=("arial", 12), fg="red", command=clear_new_search)
        self.clear_screen_btn.grid(row=0, column=1, pady=5, padx=5)

        self.copy_result_btn = tk.Button(self.seqan_button_frame, text="copy results", font=("arial", 12), fg="blue", command=copy_results)
        self.copy_result_btn.grid(row=0, column=2, pady=5, padx=5)

        self.result_frame = tk.Frame(self.seq_analyzer_window_scrol, bg="#c4c0c0")
        self.result_frame.grid(row=8, column=1)
                

        ##~~~~~~~~~Seqan-GUI-stringvars~~~~~~~~##

        self.motif_miss = tk.StringVar()
        self.motif_miss.set("pattern missing!")

        self.promoter_miss = tk.StringVar()
        self.promoter_miss.set("promoter sequence missing!")

        self.mot_pro_miss = tk.StringVar()
        self.mot_pro_miss.set("pattern and promoter missing!")

        self.dna_in_check = tk.StringVar()
        self.dna_in_check.set("nucleic acid sequence in fasta format required.")

        self.general_info = tk.StringVar()
        self.general_info.set("putative patterns will be listed if match(es) found.")


        ##~~~~~~~~~~~~~~~~~~~~~~~Seqan-Frame-labels~~~~~~~~~~~~~~~~~~~~##
        
        
        #contact frame
        self.contact_window_frame = tk.Frame(self.func_frame_seqa, width=1150, height=700, bg="#c4c0c0")

        self.contact_window_frame.rowconfigure(0, weight=1)
        self.contact_window_frame.columnconfigure((0,1,2), weight=1)

        ##~~~~~~~~~~~~~~~~~~~~~~~contact-Frame-labels~~~~~~~~~~~~~~~~~~~~##

        self.contact_window_text = tk.Label(self.contact_window_frame, 
                                                text="Contact information for error/bug report", 
                                                bg="#c4c0c0", 
                                                font=("arial", 20))
        self.contact_window_text.grid(row=0, column=1)

        self.contact_placeholder = tk.Label(self.contact_window_frame,
                                            text="",
                                            bg="#c4c0c0",
                                            font=("arial", 20))
        self.contact_placeholder.grid(row=1, column=1)

        self.contact_information = tk.Label(self.contact_window_frame, 
                                             text="Please report errors/bugs/suggestions to:\ntilli.sachslehner@gmx.at", 
                                             bg="#c4c0c0",
                                             font=("arial", 20))
        self.contact_information.grid(row=2, column=1)
    
        ##~~~~~~~~~~~~~~~~~~~~~~~contact-Frame-labels~~~~~~~~~~~~~~~~~~~~##
        
            
    ##~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~Frame_Hide_DEFINITIONS~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~##

        def change_to_home_frame():
            self.home_window_frame.pack()
            self.intro_window_frame.pack_forget()
            self.seq_analyzer_window_frame.pack_forget()
            self.contact_window_frame.pack_forget()


        def change_to_intro_frame():
            self.intro_window_frame.pack()
            self.home_window_frame.pack_forget()
            self.seq_analyzer_window_frame.pack_forget()
            self.contact_window_frame.pack_forget()


        def change_to_seq_an_frame():
            self.seq_analyzer_window_frame.pack()
            self.home_window_frame.pack_forget()
            self.intro_window_frame.pack_forget()
            self.contact_window_frame.pack_forget()


        def change_to_contact_frame():
            self.contact_window_frame.pack()
            self.home_window_frame.pack_forget()
            self.intro_window_frame.pack_forget()
            self.seq_analyzer_window_frame.pack_forget()
            
            
    ##~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~BUTTON_DEFINITIONS~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~##

        def hide_index(): # to hide indication
            self.home_index.config(bg="#e0dcdc")
            self.intro_index.config(bg="#e0dcdc")
            self.seq_analyzer_index.config(bg="#e0dcdc")
            self.contact_index.config(bg="#e0dcdc")
        
    
        def show_index(index_label, frame_change): # to hide indication and show pages of functional Frame. keywords such as frame_change are usefull for lambda applications.
            hide_index() # hide index
            index_label.config(bg="#9c9a9a") # show index
                        
                
        def exit_program():
            sequence_analyzer_root.destroy()
        
        
#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~Start of GUI~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~#

#Sequence Analyzer GUI 

sequence_analyzer_root = tk.Tk()

sequence_analyzer_root_main = Main_win_Seq_An(seq_an_root=sequence_analyzer_root)
sequence_analyzer_root.resizable(width=False, height=False)

menu_win_Seq_an = Menu_win_Seq_an()

sequence_analyzer_root.mainloop()


##--to do--##

##--priority--##

#--6 make check for motif and promoter box if empty == done 26.12.2022
#--5 insert functions (optimization!!!) == done 18.01.2023
#--4 allow copy and paste (also from output) == done 28.01.2023
#--7 BASE input check -- allow only dna bases == done 06.02.2023
#--2 more frames with Intro, about, contact. ect.. == done 11.03.2023
#--3 delete/aktualise MF output (either this or make popup window) == done 18.03.2023
#--1 combine text output of motif search or make copy button which copies the results to clipboard == done 06.04.2023

#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~#

##--no priority--##

#--1 scrollbar (rather optional) == done 26.03.2023
#--7 find the pattern reverse complement