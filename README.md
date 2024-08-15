# Date-Match

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
</head>
<body>

<h1>README: Date Matching Code</h1>

<h2>Objective</h2>
<p>The primary objective of this code is to match dates between meeting HTML files and auction files, identify the nearest dates, and calculate the differences between them. Additionally, the code aims to label these dates and add dummy variables to the initial auction files.</p>

<h2>Steps</h2>

<h3>1. Extract Meeting Dates</h3>
<p>The first step involves extracting meeting dates from the provided HTML files. These dates are crucial for subsequent steps where they are used to find the nearest auction dates.</p>

<h3>2. Labeling Meeting and Auction Dates</h3>
<p>The code proceeds to label both the extracted meeting dates and the auction dates from the previous auction files. In MySQL, primary keys act as unique identifiers for each row in a database table. Similarly, in this code, each date is assigned a unique key—'m-1' for meeting dates and 'a-2' for auction dates. This labeling allows us to track the original source of each date even after they have been matched. By clearly distinguishing between the types of dates, this process also facilitates the merging of data for further analysis.</p>

```python
//import pandas as pd

//Load the meeting dates file
meeting_file_path = 'meeting_dates_from_urls.csv'
meeting_data = pd.read_csv(meeting_file_path)

//Extract the Meeting Date column and create an index
meeting_dates = meeting_data[['Meeting Date']].drop_duplicates().reset_index(drop=True)
meeting_dates['meeting_id'] = ['m-' + str(i + 1) for i in range(len(meeting_dates))]

//Reorder the columns to have meeting_id before Meeting Date
meeting_dates = meeting_dates[['meeting_id', 'Meeting Date']]

//Save the extracted meeting dates with the index to a new CSV file
output_file = 'meeting_dates_with_ids_ordered.csv'
meeting_dates.to_csv(output_file, index=False)
```

<h3>3. Merging Dates</h3>
<p>The labeled meeting dates and auction dates are then merged. The goal of this step is to align the dates from both sources and prepare them for comparison.</p>

<h3>4. Finding the Nearest Date & Calculating Date Differences</h3>
<p>After merging, the code attempts to find the nearest auction date corresponding to each meeting date. To determine how close the meeting and auction dates are, the code uses the <code>timedelta</code> package. This package helps calculate the difference between each pair of meeting and auction dates, providing an accurate measure of the time gap.</p>

```python
# Calculate the difference in days
day_difference = (nearest_auction_date - meeting_date).days
    
# Debugging output to check the differences
print(f"Meeting ID: {meeting_id}, Auction ID: {nearest_auction_id}, Day Difference: {day_difference}")

# Check if the nearest auction date is 1 or 2 days before or after the meeting date
if -2 <= day_difference <= 2:
 marked_pairs.append((meeting_id, nearest_auction_id))

# Convert the marked pairs to a DataFrame
marked_pairs_df = pd.DataFrame(marked_pairs, columns=['Meeting ID', 'Auction ID'])
```
<h3>5. Generating All Pairs</h3>
<p>The code generates all possible pairs of meeting and auction dates, ensuring that no potential match is overlooked. This exhaustive approach is necessary for a comprehensive analysis.</p>

<h3>6. Adding Dummy Variables</h3>
<p>Finally, dummy variables are added to the initial auction files. These variables, 'Dummy Before' and 'Dummy After', serve as indicators, marking whether a corresponding meeting date exists and the proximity of the nearest date.</p>

<h2>Conclusion</h2>
<p>The code is designed to systematically extract, label, and match dates from meeting and auction files. By merging the dates and calculating their differences, the code effectively identifies the nearest auction dates for each meeting and adds valuable dummy variables to the auction files.</p>

</body>
</html>
