import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.


    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    while True:
        city = input('choose a city from the following cites: chicago, new york city, washington: ').lower()
        if city not in CITY_DATA:
                print('please provide a valid city name')
        else:
            break

    # TO DO: get user input for month (all, january, february, ... , june)
    while True:
        month = input('select a month from january to june or "all" to get all months: ').lower()
        months =['january', 'february', 'march', 'april', 'may', 'june']
        if month != 'all' and month not in months:
            print('please provide a valid month name: ')
            
        else:
            break

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    while True:
        day = input('select a day or "all" to get all days: ').lower()
        days = ['saturday', 'sunday', 'monday', 'tuesday', 'wednesday', 'thurthday', 'friday']
        if day != 'all' and day not in days:
            print('please provide a valid day name: ')
        else:
            break

    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.read_csv(CITY_DATA[city])
    
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.dayofweek
    df['hour'] = df['Start Time'].dt.hour
    print("here 2")
    print(df)
    
    if month != 'all':
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1
        df = df[df['month'] == month]
    print("here 3")
    print(df)
    
    if day != 'all':
        days= ['monday','tuesday','wednesday','thursday','friday','saturday','sunday']
        day = days.index(day)
        df = df[df['day_of_week'] == day]

    print("here 4")
    print(df)
    return df



def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    common_month = df["month"].mode()[0]
    print(common_month)
    # TO DO: display the most common day of week
    common_day = df["day_of_week"].mode()[0]
    print(common_day)
    # TO DO: display the most common start hour
    common_hour=df["hour"].mode()[0]
    print(common_hour)
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    start_station=df["Start Station"].mode()[0]
    print(start_station)

    # TO DO: display most commonly used end station
    end_station=df["End Station"].mode()[0]
    print(end_station)
    # TO DO: display most frequent combination of start station and end station trip
    combined_station=df["Start Station"] + df["End Station"]
    print(combined_station)
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_duration=df["Trip Duration"].sum()
    print(total_duration.sum)
    # TO DO: display mean travel time
    total_duration=df["Trip Duration"].mean()
    print(total_duration.mean)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    user_type = df["User Type"].value_counts()
    print(user_type)
    # TO DO: Display counts of gender
    
    print("the count of user type")
    if "Gender" in df.columns :
        gender=df["Gender"].value_counts()

    # TO DO: Display earliest, most recent, and most common year of birth
    earliest_year_birth=df["Birth Year"].min()
    most_recent_year=df["Birth Year"].max()
    most_common_year=df["Birth Year"].mode()[0]
    print(earliest_year_birth)
    print(most_recent_year)
    print(most_common_year)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def main():
    while True:
        city,month,day = get_filters()
        df =load_data(city, month, day)

        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
    main()


    
    
    
