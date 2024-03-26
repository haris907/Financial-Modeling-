import pandas as pd
from dataclasses import dataclass


@dataclass
class ModelInputs:
    starting_salary: int = 60000
    savings_rate: float = 0.25
    interest_rate: float = 0.05
    promos_raise: float = 0.15
    promos_every_n_year: int = 5
    desired_cash: int = 1500000
    inflation_rate_yearly: float = 0.02
    yearly_spendings: float = 0.0
    one_of_withdrawal: int = 1000

model_data = ModelInputs()
data = model_data
data

year = 0
starting_wealth = 0

def net_salary_at_year(data, year):

# Add promotions effect to salary
    num_promos = int(year/data.promos_every_n_year)
    cost_of_living = data.starting_salary*(1+data.inflation_rate_yearly)**year
    promotion_factor = (1+data.promos_raise)**num_promos
    salary_t = cost_of_living*promotion_factor
    
# Subtract Annual spendings    
    net_salary_t = salary_t * (1-data.yearly_spendings)
    return net_salary_t

net_salary_at_year(data, year)
def cash_saved_during_year(data, year):
    salary = net_salary_at_year(data, year)
    cash_saved = salary * data.savings_rate
    return cash_saved
prior_wealth = 0
def wealth_at_year(data, year, prior_wealth):
    
    cash_saved = cash_saved_during_year(data, year)
    wealth = prior_wealth*(1+data.interest_rate) + cash_saved
    return wealth
wealth_at_year(data, year, prior_wealth)
prior_wealth = 0
wealth = 0
year = 0

#using while loop, to stop function when the resired cash collected
while wealth < data.desired_cash:
    year = year + 1
    wealth = wealth_at_year(data, year, prior_wealth)
    print(f' During year {year}, Total wealth is ${wealth:,.0f}.')
    
    prior_wealth = wealth
def years_to_retirement(data):

    prior_wealth = 0
    wealth = 0
    year = 0

    #using while loop, to stop function when the resired cash collected
    # Also counter one of withdrawal
    while (wealth - data.one_of_withdrawal) < (data.desired_cash):
        year = year + 1
        wealth = wealth_at_year(data, year, prior_wealth)
        prior_wealth = wealth
        
    print(f' It will take {year} years to retire.')
    return year    

years_to_retirement(data)
