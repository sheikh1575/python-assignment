# python-assignment
from data_collection import get_user_data
from raw_data import read_data
from group_data import group_data, draw_histogram
from statisticsmodulle import compute_statistics

def main():
    menu = """***** Grouped Data Application *****\n"""
    menu += "Menu\n"
    menu += "1- Enter Data and Save in CSV\n"
    menu += "2- Show Statistics\n"
    menu += "3- Draw Histogram\n"
    menu += "4- Exit\n"
    menu += "Enter your Choice (1..4): "

    while True:
        choice = input(menu).strip()
        
        if choice == '1':
            print("\nEnter data and Save in CSV file.\n")
            get_user_data()
        
        elif choice == '2':
            print("\n --- Show the Statistics here---\n")
            data = read_data()
            if data:
                class_width = float(input("Enter class width: "))
                grouped_df, frequency, midpoints = group_data(data, class_width)
                stats_df = compute_statistics(data, grouped_df, frequency, midpoints)
                print(stats_df)
                stats_df.to_csv("statistics.csv", index=False)
                print("â Statistics saved to statistics.csv")
        
        elif choice == '3':
            print("\n --- Draw Histogram here---\n")  
            data = read_data()
            if data:
                class_width = float(input("Enter class width: "))
                grouped_df, _, _ = group_data(data, class_width)
                draw_histogram(grouped_df, class_width)
        
        elif choice == '4':
            print("\nExiting .....\n")
            break
        else:
            print("\nInvalid input, Try again.\n")

if __name__ == "_main_":
    main()
    Data Collection
    import pandas as pd

def get_user_data():
    print("Enter your continuous numerical data separated by commas (e.g., 12, 34.5, 22):")
    user_input = input("→ ").strip()
    try:
        values = [float(x) for x in user_input.split(",") if x]
        df = pd.DataFrame(values, columns=["Value"])
        df.to_csv("raw_data.csv", index=False)
        print(" Data saved to raw_data.csv")
    except ValueError:
        print(" Invalid input. Please enter numbers only, separated by commas.")
        Raw data
        import pandas as pd
import os

def read_data():
    if not os.path.exists("raw_data.csv"):
        print("⚠ No raw_data.csv file found. Please enter data first.")
        return []
    df = pd.read_csv("raw_data.csv")
    return df['Value'].tolist()
    Group data
    import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

def group_data(data, class_width):
    min_val, max_val = min(data), max(data)
    bins = np.arange(min_val, max_val + class_width, class_width)
    frequency, bin_edges = np.histogram(data, bins=bins)
    
    midpoints = [(bin_edges[i] + bin_edges[i+1]) / 2 for i in range(len(bin_edges)-1)]
    freq_times_mid = [frequency[i] * midpoints[i] for i in range(len(frequency))]
    
    grouped_df = pd.DataFrame({
        'Classes': [f'{bin_edges[i]} - {bin_edges[i+1]}' for i in range(len(bin_edges)-1)],
        'Frequency': frequency,
        'Midpoint': midpoints,
        'Freq * Mid': freq_times_mid
    })
    
    return grouped_df, frequency.tolist(), midpoints

def draw_histogram(grouped_df, class_width):
    plt.bar(
        grouped_df['Midpoint'], 
        grouped_df['Frequency'], 
        width=class_width,
        color='green',
        edgecolor='black',
        alpha=0.6
    )
    plt.xlabel("Midpoint")
    plt.ylabel("Frequency")
    plt.title("Histogram of Grouped Data")
Statistics Module
import statistics
import pandas as pd

def compute_statistics(data, grouped_df, frequency, midpoints):
    try:
        total_freq = sum(frequency)
        sum_fx = sum([f * m for f, m in zip(frequency, midpoints)])
        mean = sum_fx / total_freq

        median = statistics.median(data)
        try:
            mode = statistics.mode(data)
        except statistics.StatisticsError:
            mode = "Multiple Modes"

        variance = statistics.variance(data)
        std_dev = statistics.stdev(data)

        max_freq_index = frequency.index(max(frequency))
        modal_class = grouped_df.iloc[max_freq_index]['Classes']

        stats_df = pd.DataFrame({
            'Measure': ['Mean', 'Median', 'Mode', 'Variance', 'Standard Deviation', 'Modal Class'],
            'Value': [mean, median, mode, variance, std_dev, modal_class]
        })

        return stats_df

    except Exception as e:
        print(f"❌ Error in computing statistics: {e}")
        
   Requirements.txt
       
pandas
numpy
matplotlib
        
