
<br />
<p align="center">

  <h3 align="center">Heroes Of Pymoli</h3>

  <p align="center">
     Performing data analysis using pandas
    <br />
    <a href="https://github.com/HsuChe/pandas-challenge"><strong>Project Github URL »</strong></a>
    <br />
    <br />
  </p>
</p>



<!-- ABOUT THE PROJECT -->
## About The Project

![hero image](https://github.com/HsuChe/pandas-challenge/blob/d552ad43a81eb152fc208a1a150dde6396f97a4f/images/fantasy-3077928_1920.jpg)

Sales analysis is critical for business decisions. In this homework, we will be using pandas library to analyze in game purchase data from Heroes of Pymoli.

Features of the dataset:
* The in-game purchases dataset contains the following columns:
    * Purchase ID: **Unique ID for each in-game purchase**
    * SN: **The screen name of the purchasee**
    * Age: **The age of the purchasee**
    * Gender: **The gender of the purchasee**
    * Item ID: **Unique ID for the item in the transaction**
    * Item Name: **The name of the item in the transaction**
    * Prices: **The price of the transaction**

* The dataset is in the csv file format with delimiter of comma.

* Download Dataset click [HERE](https://github.com/HsuChe/pandas-challenge/blob/0f628f032da5f551f4000b80c5b4ccd4dd77c3ab/HeroesOfPymoli/Resources/purchase_data.csv)

The homework will present various business indicator to generate. The following information will be the focus for analysis.

* Unique Player Count.
* Overall purchasing analysis.
* Player and purchase analysis based on gender.
* Player and purchase analysis based on age.
* Top spender.
* Top performing item based on transaction quantity and total price.

<!-- GETTING STARTED -->
## Processing the csv and make it into a easier to use dataset.

Open the csv with csv.reader() and using a for loop to iterate through each row and adding them to a list.

* For loop to add to list
  ```sh
  csv_list = []
  for row in csv_read:
    csv_list.append(row)
  ```

After generating the csv list, find the total number of months being accounted for. To do this, we will find the index of the rows.

* Finding the total month accounted for.
  ```sh
  total_month = len(list)
  ```

### Finding the total net profit/loss

To find the totla net profit / loss, we will be using a loop that will iterate through each row in the data list and add up all of the values from loss/profit columns

* Total Profit Function:
  ```sh
  def total_profit(list):
    net = 0
    for month in range(1,len(list)):
        net = net + int(list[month][1])
    return net  
  ```

### Average month to month change.

Next, we find the average change month to month. The difference between the months will be put into a list.

* Headers for generated columns
  ```sh
  def month_change(list):
    change_list = []
    for month in range(1, len(list)-1):
        current_price, next_price = int(list[month][1]), int(list[month + 1][1])
        change = next_price - current_price
        change_list.append(change)
    return change_list
  ```

## Calculating the avearge change in profit / loss

The average change in profit / loss is the sum of the change list divided by the length of the change list. 

* Avearge change in profit and losses month to month.  
    ```sh
    def average_profit(change_list):
            average_change = sum(change_list) / len(change_list)
            average_change = round(average_change)
        return average_change
    ```

## Calculating the greatest increase and decrease in profit / loss month to month

The great increase and decrease is calculated by calculating the max and min of the change list and while referencing their index location to find the months that these changes happened. 

* Finding the highest increase and decrease in profit / loss in the dataset.
  ```sh
  def greatest_change(list):
    change_list = month_change(list)
    header_loc = 1
    month_loc = 1
    # find the highest and lowest monthly changes in the monnthly changes list
    greatest_increase, greatest_decrease = max(change_list), min(change_list) 
    # find the specific date that the highest and lowest monthly changes take place in.
    gi_month = list[change_list.index(greatest_increase) + header_loc + month_loc][0]
    gd_month = list[change_list.index(greatest_decrease) + header_loc + month_loc][0]
    return gi_month, gd_month, greatest_increase, greatest_decrease
  ```

The fact that our opening price will be extracted after the current iteration of unique value is calculated, we would have to calculate the price change year on year as well as the percentage change before updating a new opening price from the conditional.

* Store Yearly Change and Yearly Percent Change to memory
  ```sh
  YearlyChange = ClosingPrice - OpeningPrice
  ```

## Generate the summary table as a string.

We for variables for each of the information we calculated before and map them into a string. 

* Mapping the calculated value into the string. 
  ``` sh
  def summary_table(list):
    # Define the variables that will be used inn the summary table f' string
    total_month = len(list)
    net = total_profit(list)
    average_change = average_profit(month_change(list))
    gi_month, gd_month, greatest_increase, greatest_decrease = greatest_change(list)
    report = f'''
    Financial Analysis
    ----------------------------
    Total Months: {total_month}
    Total: {net}
    Average Change: ${average_change}
    Greatest Increase in Profits: "{gi_month} ({greatest_increase})"
    Greatest Decrease in Profits: "{gd_month} ({greatest_decrease})"
    '''
    # return the string
    return report
  ```

## Generating the text file and terminal message with the summary table.

To generate the text file, we will be writing the string information into it.

* Writing the information into a text file
  ```sh
  def analysis_gen(string, path):
    with open (path,"w") as file1:
            file1.write(string)
  ```

## After everything is generated, we complete the PyBank portion of the assignment.

<br>
<br>
<br>

<!-- ABOUT THE PROJECT -->
## About The Project

![hero image](https://github.com/HsuChe/python-challenge/blob/f3e0d54ab0d2365f8a35dd1a83ef981b05c49644/Image/PyPoll_Hero.jpg)

Visualizing and processing voting data is how we can find our winner and calculate critical descriptions through analyzing polling data. 

Features of the dataset:
* Voter ID: **The unique ID that each vote is identified by**
* County: **Name of the county where the vote was casted**
* Candidate: **Name of the candidate the vote was casted for**

* Download Dataset click [HERE](https://github.com/HsuChe/python-challenge/blob/f3e0d54ab0d2365f8a35dd1a83ef981b05c49644/PyPoll/Resources/election_data.csv)

The homework is interested generating a few specific items for the summary table.

* Total of votes casted
* Percentage of the total vote each candidate received.
* Winner of the election.

<!-- GETTING STARTED -->
## Processing the csv and make it into an dataset that is easier to manipulate.

Open the csv with csv.reader() and using a for loop to iterate through each row and adding them to a list.

* For loop to add to list
  ```sh
  csv_list = []
  for row in csv_read:
    csv_list.append(row)
  ```

After generating the csvlist, find the total number of votes being accounted for. To do this, we will find the index of the rows.

* Finding the total votes accounted for.
  ```sh
  total_vote = len(list)
  ```

### Find information for each candidate

## Preprocessing information and putting them into a dictionary.

To find the specific information for each candidate, the first step is to find the list of unique candidates, to do this, we can generate a list and append each name into a list when a new iteration appears.

* Unique candidate function:
  ```sh
  def UniqueCandidate(datalist):
    UniqueList = []
    for vote in range(1, len(datalist)):
        if datalist[vote][2] not in UniqueList:
            UniqueList.append(datalist[vote][2])
    return UniqueList
  ```

### Finding out the amount of votes each candidate received.

We can find out the amount of votes each candidate received based on unique candidate names.

* Candidate vote counter:
  ```sh
  def CandidateCounter(candidate, datalist):
    CandidateCount = 0
    for vote in datalist:
        if vote[2] == candidate:
            CandidateCount += 1
    return CandidateCount
  ```

## Find the winner and total votes. 

To find the winner, we will iterate through each candidate within unique candidate information and then find the name through index of the maximum vote count. To do this, we will first create a dictionary with unique candidate names and add voting information based on candidates to the dictionary. 

* Generating and adding candidate voting information to dictionary
    ```sh
    CandidateInfo = {}
    for candidate in UniqueCandidate(datalist):
        CandidateCount = CandidateCounter(candidate,datalist)
        PercentVote = (CandidateCount/totalvote)*100
        StringConvert = "%.3f" % PercentVote + "%"
        CandidateInfo[candidate] = [CandidateCount,StringConvert]
    ```

* Comparing candidate vote count to find the winner  
    ```sh
    def winner(CandidateDict):
        winner = ''
        winning_vote = 0
        for candidate in CandidateDict:
            if CandidateDict[candidate][0] > winning_vote:
                winning_vote = CandidateDict[candidate][0]
                winner = candidate
        return winner
    ```
* Putting the rest of the information into the dictionary. 
    ```sh
    CandidateInfo['Winner'] = winner(CandidateInfo)
    CandidateInfo['Total'] = totalvote
    ```

## Generate the summary table as a string.

We for variables for each of the information we calculated before and map them into a string. 

* Mapping the calculated value into the string. 
  ``` sh
  def parser(CandidateDict):
    string = f'''
        Election Results
        -------------------------
        Total Votes: {CandidateDict['Total']}
        -------------------------
        Khan: {CandidateDict['Khan'][1]} ({CandidateDict['Khan'][0]})
        Correy: {CandidateDict['Correy'][1]} ({CandidateDict['Correy'][0]})
        Li: {CandidateDict['Li'][1]} ({CandidateDict['Li'][0]})
        O'Tooley: {CandidateDict["O'Tooley"][1]} ({CandidateDict["O'Tooley"][0]})
        -------------------------
        Winner: {CandidateDict['Winner']}
        -------------------------
        '''
    return string
  ```

## Generating the text file and terminal message with the summary table.

To generate the text file, we will be writing the string information into it.

* Writing the information into a text file
  ```sh
  def analysis_gen(string, path):
    with open (path,"w") as file1:
            file1.write(string)
  ```

## After everything is generated, we complete the PyPoll portion of the assignment.
