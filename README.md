
![SEIR Model Diagram](https://upload.wikimedia.org/wikipedia/commons/3/3d/SEIR.PNG)


## Dependencies

You need [Python 3](https://www.python.org/downloads/) and [NumPy](https://numpy.org/install/). That's it.

*Note: This was built and tested on Python 3.8.0 and NumPy 1.19.2. It's possible that an older/newer versions may not be compatible.*

## Setup

1. Make sure you have [Python 3](https://www.python.org/downloads/) and [NumPy](https://numpy.org/install/) installed.
2. Clone our repository: ```git clone https://github.com/youyanggu/yyg-seir-simulator.git```
3. You are now ready to run our simulator! See sample usage below.

## Usage

To view a list of supported countries, states, and subregions, check [covid19-projections.com](https://covid19-projections.com/#view-projections).

#### List all command line flags/features:
```
python run_simulation.py --help
```

### Basic Usage

Note: `-v` means verbose mode. Remove the flag and you will get fewer print statements.

#### US simulation:
```
python run_simulation.py -v --best_params_dir best_params/latest --country US
```

#### US state simulations:
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --region NY
python run_simulation.py -v --best_params_dir best_params/latest --country US --region AZ
python run_simulation.py -v --best_params_dir best_params/latest --country US --region TX
```

#### US county simulations:
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --region CA --subregion "Los Angeles"
python run_simulation.py -v --best_params_dir best_params/latest --country US --region NY --subregion "New York City"
python run_simulation.py -v --best_params_dir best_params/latest --country US --region FL --subregion Miami-Dade
```

#### Global country simulations:
```
python run_simulation.py -v --best_params_dir best_params/latest --country Brazil
python run_simulation.py -v --best_params_dir best_params/latest --country Sweden
python run_simulation.py -v --best_params_dir best_params/latest --country "United Kingdom"
```

#### Canadian province simulation:
```
python run_simulation.py -v --best_params_dir best_params/latest --country Canada --subregion Quebec
python run_simulation.py -v --best_params_dir best_params/latest --country Canada --subregion Ontario
python run_simulation.py -v --best_params_dir best_params/latest --country Canada --subregion "British Columbia"
```

### Advanced Usage

#### Save outputs to CSV file
```
python run_simulation.py -v --best_params_dir best_params/latest --country US  --save_csv_fname us_simulation.csv
```

#### Use a custom end date
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --simulation_end_date 2020-11-01
```

#### Use a custom start date
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --simulation_start_date 2020-02-01
```

#### Simulate effect of quarantine
In this scenario, we show how the course of the epidemic would be different if just 20% of infected individuals immediately self-quarantine after showing symptoms, reducing their own transmission by 25%. For the remaining 80% of infected individuals, we assume normal transmission. You can see a graph of this scenario [here](https://covid19-projections.com/us-self-quarantine).

This can also be used as a proxy to simulate the effect of mask-wearing: if 20% of individuals wear masks, thereby reducing the overall transmission rate by 25%.
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --quarantine_perc 0.2 --quarantine_effectiveness 0.25
```

#### Use an older set of best parameters
By default, we use the latest set of best parameters in `best_params/latest`, but you can also specify a different directory.
```
python run_simulation.py -v --best_params_dir best_params/2020-06-21 --country US
```

#### Use the top 10 best parameters rather than the mean
The four choices are: mean (default), median, top, top10
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --best_params_type top10
```

### Setting Parameters

We also provide support to customize your own parameters using the `--set_param` flag. The flag takes a pair of values: the name of the parameter to be changed and its value. You can use the flag multiple times.

See the available parameter options in the [next section](#parameters).

#### Set initial R_0 to 2.0
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param INITIAL_R_0 2.0
```

#### Set initial R_0 to 2.0 and lockdown R_t to 0.9
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param INITIAL_R_0 2.0 --set_param LOCKDOWN_R_0 0.9
```

#### Set the reopen date to June 1, 2020
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param REOPEN_DATE 2020-06-01
```

#### Assume no reopening
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param REOPEN_R_MULT 1
```

#### Use a custom infection fatality rate (IFR)
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param MORTALITY_RATE 0.008
```

### Changing parameters

Sometimes you don't want to replace an existing parameter, but rather just change it. The `--change_param` flag takes a pair of values: the name of the parameter to be changed and the amount to change the value by. You can use the flag multiple times.

See the available parameter options in the [next section](#parameters).

#### Increase initial R_0 by 0.1
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --change_param INITIAL_R_0 0.1
```

#### Increase initial R_0 by 0.1 and decrease lockdown R_t by 0.1
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --change_param INITIAL_R_0 0.1 --change_param LOCKDOWN_R_0 -0.1
```

#### Increase reopening date by 7 days
This simulates what would happen if people in the US reopened one week later.
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --change_param REOPEN_DATE 7
```

#### Decrease inflection date by 7 days
This simulates what would happen if people in the US started social distancing one week earlier. You can see a graph of the results [here](https://covid19-projections.com/us-1weekearlier).
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --change_param INFLECTION_DAY -7
```

#### Set initial_R_0 to 2.2
You can also set the R_0 using `---set_param`, and then change it via `--change_param`. See below for an alternate way to set the initial R_0 to 2.2:
```
python run_simulation.py -v --best_params_dir best_params/latest --country US --set_param INITIAL_R_0 2.0 --change_param INITIAL_R_0 0.2
```

### Using Your Own Parameters
Don't want to use our parameter set? Want to run a simulation for a region/country we do not support? No problem! You can simply modify `run_simulations.py` (in the `main()` function) and specify your own default parameters (list of parameters [below](#parameters)). Afterwards, you can simply run:
```
python run_simulation.py -v
```

Note: You can still use the `--set_param` and `--change_param` flags from above, as well as the other flags explained in [Advanced Usage](#advanced-usage).

You are also welcome to change the default parameters in [`fixed_params.py`](fixed_params.py), though we would recommend caution when doing so as many of those parameters have already been optimized.

## Parameters

There are several parameters that we need to provide the simulator before it can run. We describe them below.

*Note: All reproduction number parameters are the R values before population immunity is applied. For example, if the `REOPEN_R` in New York is 1.2 and 20% of the population is infected. then the "true" Rt is 1.2 * 0.8 = 0.96.*

#### `INITIAL_R_0`

This is the initial basic reproduction number (R_0). This value is usually between 0.8-6. Read more about our R_t estimates [here](https://covid19-projections.com/about/#effective-reproduction-number-r).

#### `LOCKDOWN_R_0`

This is the post-mitigation effective reproduction number (R_t). This value is usually between 0.3-1.5.

#### `INFLECTION_DAY`

This is the date of the inflection point when `INITIAL_R_0` transitions to `LOCKDOWN_R_0`. This is usually an indicator of when people in a region began social distancing. For example, in New York, the inflection day is around [March 14](https://twitter.com/youyanggu/status/1262881629376180227).

#### `RATE_OF_INFLECTION`

This is the rate at which the `INITIAL_R_0` transitions to `LOCKDOWN_R_0`. A number closer to 1 indicates a faster transition (i.e. faster lockdown), while a number closer to 0 indicates a slower transition. For most region, this value is between 0.15-0.5. The more localized a region, the higher the rate of inflection. So for example, we estimate New York City has a `RATE_OF_INFLECTION` between 0.4-0.5, while the US as an entire country has a `RATE_OF_INFLECTION` of approximately 0.2-0.3.

#### `LOCKDOWN_FATIGUE`

We incorporate a lockdown fatigue multiplier that is applied to the R_t. This is to simulate the increase in mobility after several weeks of lockdown, despite the order not being lifted. The default value is 1. If this value is greater than 1, then it will contribute to an increase in infections in the weeks following the lockdown/mitigation. We do not use this for the majority of our projections.

#### `DAILY_IMPORTS`

To begin our simulation, we must initialize / bootstrap our model with an initial number of infected individuals. Any epidemic in a region must begin with a number of imported individuals. This value gradually goes to 0 as community spread overtakes imported cases as the major source of spread. This value typically ranges from 10-1000.

#### `MORTALITY_RATE`

This is the initial estimate of the infection fatality rate (IFR). This value is usually between 0.005-0.0125. Read more about our IFR estimates [here](https://covid19-projections.com/about/#infection-fatality-rate-ifr).

#### `REOPEN_DATE`

This is the date we estimate the region to reopen. Read more about our reopening assumptions [here](https://covid19-projections.com/about/#social-distancing).

#### `REOPEN_SHIFT_DAYS`

Even though some regions open on the same date, infections can begin increasing faster or slower than we'd normally expect. We use the `REOPEN_SHIFT_DAYS` to account for that. For example, a value of 7 means that we are shifting the `REOPEN_DATE` to be 7 days later, while a value of -7 means we are shifting it to be 7 days earlier. The default value is 0.

#### `REOPEN_R`

(Added 2020-07-22) This is the maximum reopening R_t value. It takes roughly one month after the reopening to reach this value.

#### `REOPEN_INFLECTION`

(Added 2020-07-22) Similar to the `RATE_OF_INFLECTION` parameter froma bove, this value determines the rate of inflection from `LOCKDOWN_R_0` to `REOPEN_R`, as well as from `REOPEN_R` to `POST_REOPENING_EQUILIBRIUM_R` (see below). This value is usually between 0.15-0.35. The lower the value, the slower (and longer) the transitions. Hence, large countries usually have a lower value while US states typically have a value between 0.25-0.35. Read more about our post-reopening assumptions [here](https://covid19-projections.com/about/#post-reopening).

#### `POST_REOPENING_EQUILIBRIUM_R`

(Added 2020-07-19) This is the equilibrium R_t value after sufficient time has passed from the initial reopening. By default, this is set to 1, but can be adjusted. After accounting for immunity (which lowers R_t), the equilibrium R_t is likely between 0.85 and 1.

#### `FALL_R_MULTIPLIER`

(Added 2020-07-07) We are incorporating a potential for a [fall wave](https://covid19-projections.com/about/#fall-wave) as a result of school reopenings and the beginning of influenza season. This is the daily multiplier applied to the R value. As of July/August, because it is still too early to learn this value, this is sampled randomly from a triangular distribution with mode 1.001.

## Other Relevant Repositories

- [Main COVID-19 Repository](https://github.com/youyanggu/covid19_projections)
- [Infections Estimates](https://github.com/youyanggu/covid19-infection-estimates-latest)
- [Historical CDC Vaccination Data](https://github.com/youyanggu/covid19-cdc-vaccination-data)
- [COVID-19 datasets](https://github.com/youyanggu/covid19-datasets)
- [Model Evaluations](https://github.com/youyanggu/covid19-forecast-hub-evaluation)

