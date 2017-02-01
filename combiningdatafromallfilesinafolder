import pandas as pd
import os
from math import log

loopset = []

def Tradeprice_persecond(infile):

    # This reads the csv file then the second line sets it as a panda dataset:
    SPY_Tradedata = pd.read_csv(infile, index_col=['time'])

    # I then replicate the dataset to use the copy to manipulate:
    SPY_Tradedatacopy = SPY_Tradedata
    
    # These create lists for the trade prices and for the timeset:
    prices = []
    timeset = []
    dateset = []
    tradeVolset = []
    basepriceset = []
    
    # This is the baseline time (when the minutes are released) for the cumulative log returns calculations:
    basetime = 140000000
        
    
    # This presents a loop for the ongoing trade price:
    for hourvar in range(80000000, 160000000, 10000000):
        for minvar in range(0, 6000000, 100000):
            for secvar in range(0, 60000, 1000):
                timevar = hourvar + minvar + secvar            
                price = SPY_Tradedatacopy.TradePrice.asof(timevar)
                dates = SPY_Tradedatacopy.date.asof(timevar)
                tradeVolume = SPY_Tradedatacopy.TradeVol.asof(timevar)
                prices.append(price)
                timeset.append(timevar)
                dateset.append(dates)
                tradeVolset.append(tradeVolume)
                if timevar == basetime:
                    baseprice = SPY_Tradedatacopy.TradePrice.asof(timevar)

    for hourvar in range(80000000, 160000000, 10000000):
        for minvar in range(0, 6000000, 100000):
            for secvar in range(0, 60000, 1000):
                basepriceset.append(baseprice)

    
    # This creates the pandas data frame with column names "Timecol" and "Pricecol":
    loopdata = pd.DataFrame(columns=['Datecol', 'Timecol', 'Pricecol', 'Volcol', 'basepricecol'],index=range(len(prices)))

    # These incorporate the data from the loop
    loopdata['Datecol'] = dateset
    loopdata['Timecol'] = timeset
    loopdata['Pricecol'] = prices
    loopdata['Volcol'] = tradeVolset
    loopdata['basepricecol'] = basepriceset
    
    # This creates a copy of loopdata as loopdatacopy
    loopdatacopy = loopdata    
    
    # This creates the lag variable of price
    # loopdatacopy['LagPricecol'] = loopdatacopy['Pricecol'].astype(float).shift(1)
    
    # This command incorporates the new loopdata dataframe as part of a set of panda dataframes:
    loopset.append(loopdatacopy)    
   
# This is a function used to calculate the cumulative log returns
def cumreturncalc(row):
    priceperi = row['Pricecol']
    baseperi = row['basepricecol']
    log_value = 100*log(priceperi/baseperi)
    return log_value

# This is the directory for the 
folder = 'C:\Users\Raul\Dropbox\FOMC Sentiment Analysis using Python\Data1'

# This is a for-loop for running the function through all the files in the folder
for filename in os.listdir(folder):
    infilename = os.path.join(folder, filename)
    if not os.path.isfile(infilename): continue

    base, extension = os.path.splitext(filename)
    fileholder = open(infilename, 'r')
    Tradeprice_persecond(fileholder)

# This concatenates the list of dataframes in the set "loopset". It also ignores the indices of each individual dataframe:
AllTrades_persec = pd.concat(loopset, ignore_index=True)
# print AllTrades_persec

AllTrades_persec['cumreturns'] = AllTrades_persec.apply (lambda row: cumreturncalc (row),axis=1)
# print AllTrades_persec
