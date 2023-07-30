# Shivansh
Data Wizard:Automated Data Preprocessing 
import tkinter as tk, pandas as pd, io from tkinter import ttk, filedialog, messagebox

from tabulate import tabulate

df = None

preprocessing_dict = {}

class DataWizard:

def _init_(self, root):

self.root root

self.root.title("Data Wizard: Automated Data Preprocessing")

self.root.geometry("800x600") self.root.configure(bg='#005792

')

#Top Layout with title self.title_label = tk.Label(self.root, text="Data Wizard: Automated self.title_label.pack(pady=20) self.title_label.configure (bg='#005792', fg='#7dd8c5')

# Middle Layout with table frame

self.table_frame = tk. Frame(self.root, bd=2, relief=tk. GROOVE)

self.table_frame.pack (fill=tk.BOTH, expand=True, padx=20, pady=10)

# Create a treeview widget to display table self.tree = ttk. Treeview(self.table_frame, selectmode='browse')

self.tree.pack(side-tk.LEFT, fill=tk. BOTH, expand=True)

# Add scrollbar to the treeview widget

scrollbar = ttk.Scrollbar (self.table_frame, orient="vertical", command=sel

scrollbar.pack(side=tk. RIGHT, fill=tk.Y)

self.tree.configure(yscrollcommand=scrollbar.set)

# Bottom Layout with Upload CSV file Label and button self.bottom_frame = tk. Frame(self.root, bd=2, relief=tk.GROOVE) self.bottom_frame.pack (fill=tk.X, padx=20, pady=10) self.bottom_frame.configure (bg='#C3C3C3')

self.upload_label = tk. Label (self.bottom_frame, text="Upload CSV File", for

self.upload_label.pack(side-tk. LEFT, padx=10, pady=10)

self.upload_label.configure (bg='#C3C3C3', fg='#0b1956')

self.upload_button= tk.Button(self.bottom_frame, text="Browse", command=s self.upload_button.pack(side=tk.RIGHT, padx=10, pady=10)

self.new_frame = tk. Frame(self.root, bd-0, relief=tk.GROOVE) self.new_frame.pack (fill=tk.X, padx=20, pady=10) self.new_frame.configure (bg='#005792')

#Add button to open a new window

self.new_window_button= tk.Button(self.new_frame, text="Next", command-se self.new_window_button.pack(side-tk. RIGHT, padx=10, pady=10)

def upload_csv(self):

global df

# Open file dialog to select a CSV file file_path = filedialog.askopenfilename (filetypes=[("CSV files", "*.csv")])

if file_path:

#Read CSV file as pandas dataframe df = pd.read_csv(file_path)

# Set the columns of the treeview widget based on the dataframe column: self.tree[ "columns"] = list(df.columns)

# Set column headings and widths based on the dataframe for col in df.columns:

self.tree.heading (col, text-col.title())

self.tree.column(col, width= len(str(df[col].max())), anchor = tk.c

#Insert data into treeview widget

for i, row in df.iterrows(): self.tree.insert("", "end", values-list(row))

#Change the upload Label to the filename self.upload_label.config(text-file_path)

# Add horizontal scrollbar to the treeview widget

horizontal_scrollbar ttk.Scrollbar(self.table_frame, orient="horizon

horizontal_scrollbar.pack(side=tk.BOTTOM, fill=tk.X)

self.tree.configure (xscrollcommand-horizontal_scrollbar.set)

def open_new_window(self): global df

new_window= tk. Toplevel() new_window.title("Data Wizard: Automated Data Preprocessing")

new_window.configure(bg='#005792)

# Set the window size to full screen

new_window.state('zoomed')

# Create a text widget to display the info

text_widget = tk. Text (new_window, bd=2, relief tk.GROOVE, width=70, height text_widget.grid(row=0, column=0, padx=15, pady-10, sticky="nw")

text_widget.configure(bg='#f6f6f6') #Redirect the output of df. info() to the text widget

with io.StringIO() as stream:

df.info(buf-stream) output stream.getvalue()

text_widget.Insert(tk. END, output)

# Make the text widget uneditable text_widget.config(state="disabled")

# Dictionary of Data-Preprocessing Operations

preprocessing operations = { 1: "Remove Rows with Null Values",

2: "Replace Null Values by Mean",

3: "Replace Null Values by Median"

4: "Replace Null Values by Mode", 5: "Perform One Hot Encoding",

6: "Perform Label Encoding",

7: "Perform Min Max Scaling",

8: "Perform Standardization", 9: "Find Outliers and Remove the Rows with Outliers"

}

# Dictionary of Dato-Preprocessing Operations converted to a tabular forma

table = tabulate(preprocessing operations.items(), headers = ["Sr. No.", "Pri

# Create a text widget to display the info text_widget2 = tk. Text (new_window, bd=2, relief-tk.GROOVE, width=70, heigh

text_widget2.grid(row=1, column=8, padx=15, pady=5, sticky="sw")

text_widgetz.configure(bg='#f6f6f6')

text_widget2.tag_configure('center', justify='center')

#Insert the table into the text widget

text_widget2.insert(tk. END, "List of Data Preprocessing Operations

text_widget2.insert(tk. END, table) # Make the text widget uneditable

text_widget2.config(state="disabled")

#New Frame to hold the contents

f= tk. Frame (new_window)

f.grid(row=8, column=1, padx=15, pady-18, rowspan=2, sticky="nsew")

f.configure(bg='#005792")

#Text widget to print instructions

text_widget3 = tk. Text (f, bd=2, relief-tk.GROOVE, width=80, height=6)

: \n\n")
def add_preprocessing operation():

# Clear the text widget4 before updating text_widget4.delete('1.0', tk.END)

#Get the inputs from the user

sr_num_operation int(input1.get())

column_name= input2.get()

# Call the check preprocessing operation() function

try:

check_preprocessing operation (df, sr_num_operation, column_name, p except ValueError as e:

# Show a pop-up message if there is an error

messagebox.showerror("Error", e) return

# If there are any preprocessing operations in the dictionary, print ti

if preprocessing_dict:

table2 = tabulate (preprocessing_dict.items(), headers = ["Column Nam

text_widget4.insert(tk. END, table2)

# Print only the newly added operation prev_ops = [op for ops in preprocessing_dict.values() for op in op

if prev_ops: text_widget4.insert(tk. END, "\nNew operation added for column else:

text_widget4.insert(tk. END, "No preprocessing operations added yet

def check_preprocessing operation (df, sr_num_operation, column_name, prepri

if sr_num_operation not in range(1, 10) or column_name not in df.colum

if sr_num_operation not in range(1, 10): print("Invalid operation entered. Check list for reference!") if column_name not in df.columns:

print("Column", column_name," does not exist in the DataFram

return

preprocessing operation = preprocessing operations [sr_num_operation]

if len(preprocessing_dict) >= 10: raise ValueError("Maximum number of preprocessing operations reach

if preprocessing operation == "Remove Rows with Null Values": if dffcolumn_name].isnull().values.any():

messagebox.showerror("Error", "Operation cannot be applied return

to

else: preprocessing_dict.setdefault(column_name, []).append(preproce

elif preprocessing operation in ["Replace Null Values by Mean", "Repla

if df[column_name].dtype.kind not in 'fi':

messagebox.showerror("Error", "Operation cannot be applied to

return

else: preprocessing_dict.setdefault(column_name, ]).append(preproce

elif preprocessing operation == "Perform One Hot Encoding": if df[column_name].dtype.kind not in '0': messagebox.showerror("Error", "Operation cannot be applied to return

else: preprocessing_dict.setdefault (column_name, []).append(preproce

elif preprocessing operation == "Perform Label Encoding": if df [column_name].dtype.kind not in '0':

messagebox.showerror("Error", "Operation cannot be applied to return else: preprocessing_dict.setdefault(column_name, []).append(preproce

elif preprocessing operation in ["Perform Min Max Scaling", "Perform S if df[column_name].dtype.kind not in 'fi':

messagebox.showerror("Error", "Operation cannot be applied to

return preprocessing_dict.setdefault(column_name, []).append(preproce

else:

else:

print("Invalid operation entered.")

add_button= tk.Button(inputs frame, text="Add", width=10, command=add_preprocessing
else:

preprocessing_dict.setdefault(column_name, []).append(preproce

else:

print("Invalid operation entered.")

add_button= tk.Button(inputs frame, text="Add", width=10, command=add_pre

add_button.grid(row=2, column=e, columnspan=2, pady=5)

# Button to apply the Preprocessing Logic preprocess_button = tk.Button(f, relief tk.GROOVE, text="Perform Preproces

preprocess_button.pack(side="top", fill="x", pady=10)

def preprocess_data(open_new_window):

preprocessed_window= tk. Toplevel() preprocessed_window.title("Data Wizard: Automated Data Preprocessing")

preprocessed_window.geometry("888x600")

preprocessed_window.configure(bg="#005792')

preprocessed_label= tk.Label (preprocessed_window, text="Preprocessed Data preprocessed_label.pack (side-tk.TOP, fill=tk.X, padx=10, pady-10, anchor="

# Create a frame to hold the treeview widget and scrollbar

table_frame= tk. Frame (preprocessed_window) table_frame.pack(side-tk.TOP, fill=tk. BOTH, expand=True, padx=16, pady=10)

# Create a treeview widget to display table # Create a treeview widget to display table

tree = ttk. Treeview(table_frame, selectmode="browse", height = tree.pack(side-tk. LEFT, fill=tk.BOTH, expand=True, padx=(0, 10)) 20)

# Add verticol scrollbar to the treeview widget

y_scrollbar = ttk.Scrollbar (table_frame, orient="vertical", command=tree.y

y_scrollbar.pack(side-tk. RIGHT, fill=tk.Y)

tree.configure(yscrollcommand-y_scrollbar.set)

#Add horizontal scrollbar to the treeview widget x_scrollbar = ttk.Scrollbar (table_frame, orient="horizontal", command-tree

x_scrollbar.pack (side-tk.BOTTOM, fill=tk.X, pady=(0, 10), padx (10, 0))

tree.configure(xscrollcommand=x_scrollbar.set)

def apply preprocessing operation (df, preprocessing_dict):

Apply the selected preprocessing operation on the selected column(s) o

for column_name, operation in preprocessing_dict.items():

if operation == "Remove Rows with Null Values":

df= df.dropna (subset=[column_name])

elif operation == "Replace Null Values by Mean":

df[column_name].fillna (df[column_name].mean(), inplace=True)

elif operations "Replace Null Values by Median":

df[column_name].fillna (df[column_name].median(), inplace=True) elif operation == "Replace Null Values by Mode":

mode=df[column_name].mode()[8]

df[column_name].fillna (mode, inplace=True) elif operation == "Perform One Hot Encoding":

df pd.get_dummies (df, columns=[column_name])

elif operation == "Perform Label Encoding":

from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()

df[column_name] = le.fit_transform(df[column_name])

elif operation "Perform Min Max Scaling":

from sklearn.preprocessing import MinMaxScaler scaler MinMaxScaler()

df[column_name] = scaler. fit_transform(df[[column_name]])

elif operation == "Perform Standardization":

from sklearn.preprocessing import StandardScaler scaler StandardScaler()

df[column_name] = scaler.fit_transform(df[[column_name]])

elif operation == "Find Outliers and Remove the Rows with Outliers from scipy import stats

zscore = np.abs(stats.zscore (df[column_name])) threshold = 3

df=df[(zscore threshold)] return df

preprocessed_data = apply_preprocessing operation (df, preprocessing_dict)

# Display preprocessed data in treeview widget tree["column"] = list(preprocessed_data.columns)

treef"show"] = "headings"
elif operation == "Find Outliers and Remove the Rows with Outliers from scipy import stats

zscore = np.abs(stats.zscore (df[column_name]))

threshold = 3

df = df[(zscore < threshold)]

return df

preprocessed_data = apply preprocessing operation (df, preprocessing_dict)

# Display preprocessed data in treeview widget

tree["column"] = list(preprocessed_data.columns)

tree["show"] = "headings" for column in tree["column"]:

tree.heading (column, text-column) tree.column(column, anchor="center" )

df rows = preprocessed_data.to_numpy().tolist()

for row in df rows: tree.insert("", "end", values-row)

def

export_csv(df):

file_path = filedialog.asksaveasfilename (defaultextension=".csv")

df.to_csv(file_path, index=False)

# Add button to download CSV file

download_button= tk.Button(preprocessed_window, text="Download CSV", comm

download_button.pack(side-tk.TOP, fill=tk.NONE, padx=10, pady=10, expand=T

if

name _main_":

root = tk.Tk() app Datawizard (root)

root.mainloop()
