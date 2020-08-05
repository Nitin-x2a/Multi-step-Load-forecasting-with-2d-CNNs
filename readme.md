1. The jupyter notebook contains the code used for training.

2. The file 'backend.py' contains all the functions imported into the jupyter notebook.

3. The Megawatts.csv file contains the load data of Goa from 2016 to 2019.

4. The Festival.csv file contains the festival information of Goa during the same period.

Energy in electrical form is perhaps the most efficient and the most convenient when transported in large quantities over large distances. However, unlike gas, coal, oil or hydrogen, storing electrical energy is a problem yet to be conquered. Due to this, Power generation companies, and consumer companies face economical and technical challenges in planning, operationand control. The knowledge of current and future loads can help them in overcoming these challenges.

In India, every state has a State Load Dispatch Centre (SLDC) which has to do perform Short Term Load Forecasting (STLF) to purchase power every day. Every day is divided into ninety-six slots of fifteen minutes each. The power purchase process for any slot opens twenty-hours prior to that slot, and close forty-five minutes before it. Every state has to bid their power requirement for any slot in its designated opening time-duration only.

Here, we have a trained a 2D CNN model that can make 96 forecasts at one. The electric load is a cyclic time-series, and so first we have extracted data based on the calendar information, arranged it into 4 channeled images, and then used the CNN to make forecasts on it.

The load pattern is unique for each day of the week. However, Sunday’s load pattern is very different from the pattern of other weekdays. The load pattern of festivals is closest to that of Sundays. It is because Most industries, offices, institutions remain closed on public holidays, and so the total load consumption is significantly less than that during regular weekdays. Also, the small load trend of Sundays spills over into Mondays where the load during the night hours is the lowest among all days, and the morning peak is delayed by a couple of hours. 

So, two assumptions are made for the feature selection process: 
All previous holidays are considered as Sundays, irrespective of whichever day of the week they actually are. 
Similarly, a day falling next to any holiday is regarded asa Monday, except when it did not happen to be a holiday or a Sunday itself.

We have selected load values from the previous twelve days (skipping the seventh day) to capture the recent daily trends. Load values from same day
of previous twelve weeks (past seventh day, past fourteenth day, and so on) are selected in order to capture the patterns that are unique to each weekday. Nearby lags such as 93, 94, 95, 97, 98, 99 (if we consider the lag 96, for example) also show high autocorrelation. It is expected since, in a high-
resolution time-series, the observations don’t vary drastically over small time periods. So, apart from lags that are exact multiples of 96 (which show very high autocorrelation), we also select three lags prior and three lags after them. This gives us two advantages. First, the model now has useful information about load variations in the vicinity of any lag in focus. Second, with multiple lags representing a particular day from the past, the effect of any outliers is dampened. 

The extracted data is arranged into four channels. Two channels are used for the data extracted from the past twelve days, and the other two channels
contain data from the previous twelve same days of the week. Each channel has the shape of 6x7. The row of a any given channel contains the seven values belonging to any particular day.

Below is the CNN architecture used.

![ScreenShot](https://raw.githubusercontent.com/drishtadyumna/Multi-step-Load-forecasting-with-2d-CNNs/master/arch.png)

Best are the results over a week from the test set, for Monday till Sunday. The fourth day, Thursday was a holiday.

![ScreenShot](https://raw.githubusercontent.com/drishtadyumna/Multi-step-Load-forecasting-with-2d-CNNs/master/result.png)

	
