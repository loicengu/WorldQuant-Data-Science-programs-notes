# Tabular and Python Data Structures
    ## Working with Lists
        - Square brackets []
        -values separated by commas
        -indexed from zero
        -ordered 
        -mutable ; you can add ,remove or delete elements after creation
        -Dynamic ; can shrink or expand
        -Heterogenous ; can contain dfferent types of elements from numbers,letters
        -Allows slicing to access a part of the list
        -Iterable using for and while loops
        -Flexible ; a list can conatin another list ie nesting
        -You can append an element to a list using the append method
          ie #calculate the price per m2 
            price_per_m2 = house_0_price_m2[0]/house_0_price_m2[1]
            print('price per square metre',price_per_m2)

            # Append price / sq. meter to `house_0_list`
            house_0_list.append(price_per_m2)




    ## Working with Dictionaries
        - Key - Value pair
        -Unordered
        -Mutable 
        -Dynamic
        -Keys are unique
        -Keys are immutable
        -Iterable
        -You can create a list of dictionaries ie json format
        -easy to do row-wise calcualtions but column-wise calculations are complicated

    ## Tabular and pandas dataframe
         Pandas is a data science library that allows you to organize data into dataframes.


# Data wrangling with pandas
    
    ## Fisrt step is importing the data 
       1. CSV files 
            a)using pandas 
                import pandas as pd

                df = pd.read_csv('file.csv')
            b)using csv modules
                import csv

                with open('file.csv', mode='r') as file:
                    csv_reader = csv.reader(file)
                    for row in csv_reader:
                        print(row)
        2. SQL Databases
            a) using pandas with SQLalchemy
                  import pandas as pd
                    from sqlalchemy import create_engine

                    engine = create_engine('sqlite:///example.db')
                    df = pd.read_sql('SELECT * FROM table_name', con=engine)

            b) using sqlite3
               import pandas as pd
                from sqlalchemy import create_engine

                engine = create_engine('sqlite:///example.db')
                df = pd.read_sql('SELECT * FROM table_name', con=engine)
        3. JSON files
            a) using pandas 
                import pandas as pd
                df = pd.read_json('file.json')
            b) using json module
                import json

                with open('file.json', 'r') as file:
                    data = json.load(file)
                    print(data)
        4. Excel Files
            a) using pandas
                import pandas as pd

                df = pd.read_excel('file.xlsx', sheet_name='Sheet1')
        5. Loading Parquet files
            a). using pandas
                import pandas as pd

                df = pd.read_parquet('file.parquet')
        6.loading HDF5 files 
            a). using pandas
                import pandas as pd

                df = pd.read_hdf('file.h5', 'dataset_name')
        7.XML files
            a). using pandas 
                import pandas as pd

                df = pd.read_xml('file.xml')
            b). using xml.etree.ElementTree
                import xml.etree.ElementTree as ET

                tree = ET.parse('file.xml')
                root = tree.getroot()
                for child in root:
                    print(child.tag, child.attrib)
        8. text files
            a). using basic file operations 
                with open('file.txt', 'r') as file:
                data = file.read()
                print(data)
            b).from APIS 
                import requests

                response = requests.get('https://api.example.com/data')
                data = response.json()
                print(data)

        9. Loading Data from AWS S3
               Using boto3
               import boto3

                s3 = boto3.client('s3')
                response = s3.get_object(Bucket='bucket_name', Key='file.csv')
                data = response['Body'].read().decode('utf-8')
                print(data)


        10. Google Sheets 
            using gspread
                import gspread

                gc = gspread.service_account(filename='path/to/credentials.json')
                sh = gc.open('SheetName')
                worksheet = sh.sheet1
                data = worksheet.get_all_records()
                print(data)



    ## after loading the data use shape attribute , use info to check the data types and missing values for each column.Then use the head method to see the firts few rows of the data.
       ie remove the "$" and "," characters from "price_usd" and recast the values in the column as floats.

            df1.dropna(inplace = True)
            df1["price_usd"]=(df1["price_usd"]
            #replace the $ 
            .str.replace("$", "", regex= False)
            #replace the ,
            .str.replace(",", "")
            #cast as float
            .astype(float).head())

# EDA (Exploratory Data Analysis)
  After loading and cleaning the data, We can now do exploratory data analysis.A good way to plan your EDA is by looking each column and asking yourself questions what it says about your dataset.This is where you can engage differnt visualizations to understand the data:
                            1. Histograms
                            shows the distribution of a single numerical varaiable
                                import matplotlib.pyplot as plt
                                import seaborn as sns

                                sns.histplot(data['column_name'])
                                plt.show()
                            2. Bar Plots 
                            compare categorical data
                             ie First, use the [`groupby`] method to create a Series where the index contains each state in the dataset and the values correspond to the mean house price per m<sup>2</sup> for that state. Then use the Series to create a bar chart of your results. Make sure the states are sorted from the highest to lowest mean, that you label the x-axis as `"State"` and the y-axis as `"Mean Price per M^2[USD]"`, and give the chart the title `"Mean House Price per M^2 by State"`.

                             # Group `df` by "state", create bar chart of "price_per_m2"
                                (
                                    df.groupby("state")["price_per_m2"].mean().sort_values(ascending = False)
                                .plot(kind = "bar",xlabel="State",ylabel = "Mean Price per M^2[USD]",title="Mean House Price per M^2 by State")
                                )

                            3.Box Plots
                                Show the distribution of a numerical variable across different categories.
                                sns.boxplot(x='categorical_column', y='numerical_column', data=data)
                                    plt.show()
                            4. Violin Plots
                                Combine the benefits of box plots and kernel density plots.
                                sns.violinplot(x='categorical_column', y='numerical_column', data=data)
                                plt.show()
                            5. Scatter Plots
                                Show the relationship between two numerical variables.
                                sns.scatterplot(x='numerical_column1', y='numerical_column2', data=data)
                                plt.show()
                            6. Line Plots
                                Display trends over time or ordered data.
                                sns.lineplot(x='time_column', y='value_column', data=data)
                                plt.show()
                            7. Pair Plots
                                Visualize relationships between multiple numerical variables.
                                Example using seaborn:
                                python
                                Copy code
                                sns.pairplot(data)
                                plt.show()
                            8. Heatmaps
                                Show the correlation between numerical variables.
                                Example using seaborn:
                                python
                                Copy code
                                sns.heatmap(data.corr(), annot=True, cmap='coolwarm')
                                plt.show()
                            9. Facet Grids
                                Visualize data subsets across multiple plots.
                                Example using seaborn:
                                python
                                Copy code
                                g = sns.FacetGrid(data, col='categorical_column')
                                g.map(sns.histplot, 'numerical_column')
                                plt.show()
                            10. Swarm Plots
                                Show the distribution of categorical data and detect outliers.
                                Example using seaborn:
                                python
                                Copy code
                                sns.swarmplot(x='categorical_column', y='numerical_column', data=data)
                                plt.show()
                            11. Strip Plots
                                Similar to swarm plots but can be less informative when many points overlap.
                                Example using seaborn:
                                python
                                Copy code
                                sns.stripplot(x='categorical_column', y='numerical_column', data=data)
                                plt.show()
                            12. Joint Plots
                                Display a bivariate plot along with the univariate plots.
                                Example using seaborn:
                                python
                                Copy code
                                sns.jointplot(x='numerical_column1', y='numerical_column2', data=data)
                                plt.show()
                            13. Area Plots
                                Show cumulative totals over time or other variables.
                                Example using pandas:
                                python
                                Copy code
                                data.plot.area()
                                plt.show()
                            14. Pie Charts
                                Show the proportion of categories.
                                Example using matplotlib:
                                python
                                Copy code
                                data['categorical_column'].value_counts().plot.pie()
                                plt.show()
                            15. Treemaps
                                Show hierarchical data as nested rectangles.
                                Example using squarify:
                                python
                                Copy code
                                import squarify

                                squarify.plot(sizes=data['numerical_column'], label=data['categorical_column'])
                                plt.show()
                            16. Word Clouds
                                Visualize text data by displaying the most frequent words.
                                Example using wordcloud:
                                python
                                Copy code
                                from wordcloud import WordCloud

                                text = ' '.join(data['text_column'])
                                wordcloud = WordCloud().generate(text)
                                plt.imshow(wordcloud, interpolation='bilinear')
                                plt.axis('off')
                                plt.show()




