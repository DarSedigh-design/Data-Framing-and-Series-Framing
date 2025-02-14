# Data-Framing-and-Series-Framing
import pandas as pd

def load_and_analyze_file():
    # Get file name from user
    file_name = input("Enter the filename (including extension, e.g., data.csv, data.xlsx, data.json): ")
 
  try:
        # Determine file type and read accordingly
        if file_name.endswith('.csv'):
            df = pd.read_csv(file_name)
        elif file_name.endswith('.xlsx'):
            df = pd.read_excel(file_name)
        elif file_name.endswith('.json'):
            df = pd.read_json(file_name)
        else:
            print("Unsupported file format. Please provide a CSV, Excel, or JSON file.")
            return
        
        print("\nFile loaded successfully! Here are the first 5 rows:")
        print(df.head())
        
        # Convert to Series if there's only one column
        if len(df.columns) == 1:
            series = df[df.columns[0]]
            print("\nThe file contains a single-column Series:")
            print(series.head())
        
        # Provide analysis options
        while True:
            print("\nAvailable columns:", list(df.columns))
            column_choice = input("Enter the column name you want to analyze (or type 'exit' to quit): ")
            
            if column_choice.lower() == 'exit':
                break
            
            if column_choice in df.columns:
                print(f"\nSummary statistics for {column_choice}:")
                print(df[column_choice].describe())
                print("\nUnique values:")
                print(df[column_choice].unique())
                
                # Additional analysis options
                if df[column_choice].dtype == 'object':
                    print("\nMost common values:")
                    print(df[column_choice].value_counts().head(10))
                else:
                    print("\nCorrelation with other numeric columns:")
                    print(df.corr()[column_choice])
            else:
                print("Invalid column name. Please try again.")
    
    except FileNotFoundError:
        print("Error: File not found. Please check the filename and try again.")
    except Exception as e:
        print(f"An error occurred: {e}")

if __name__ == "__main__":
    load_and_analyze_file()
