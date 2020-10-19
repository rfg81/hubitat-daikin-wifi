## API System

Daikin original API use REST.

You can use GET http request to retrive informations and POST http request to apply new settings.

Note: There are more API calls discovered at the bottom of this document

Uri                | GET | POST | desc
-------------------|-----|------|-----
/common/basic_info | X   |      | Provides Daikin account information (security issue!), software version, mac address and generic info
/common/get_remote_method | X | | Provides information about polling system
/common/set_remote_method | | X | Set information on the polling system (reduce remote time update ??)
/aircon/get_model_info | X | | Provides model informarion
/aircon/get_control_info | X | | Main Uri to request all current status parameters
/aircon/set_control_info | X | X | Main Uri to set status parameters (control almost all)
/aircon/get_sensor_info | X | | Provides information on sensors (temperatures, humidity, power consumption)
/aircon/get_timer  | X | | ?
/aircon/set_timer  | | X | ?
/aircon/get_price  | X | | ?
/aircon/set_price  | | X | ?
/aircon/get_target | X | | ?
/aircon/set_target | | X | ?
/aircon/get_week_power| X | | Provides weekly and today runtime information (in mn)
/aircon/get_week_power_ex| X | | Energy consumption per day last 7 days in thenths of a kWh
/aircon/get_week_power| X | | Provides weekly and today runtime information (in mn)
/aircon/get_week_power_ex| X | | Energy consumption per day last 7 days in thenths of a kWh
/aircon/get_year_power| X | | Provides yearly runtime information
/aircon/get_year_power_ex| X | | Energy consumption each month this and last year in thenths of a kWh
/aircon/get_program | X | | ?
/aircon/set_program | | X | ?
/aircon/get_scdltimer | X | | Provides information about on/off weekly timer
/aircon/set_scdltimer | | X | Set information about on/off weekly timer
/common/get_notify  | X | | ?
/common/set_notify  | | X | ?
/common/set_regioncode | | X | ?
/common/set_led | | X | ?
/common/reboot | X |  | reboot the AP

## Parameters

### `/aircon/set_control_info`

#### Power
param name :  **pow**

description: represents the power state of the device

value | desc
:----:|-----
  0   | OFF
  1   | ON

#### Mode
param name :  **mode**

description: represents the operating mode

value | desc
:----:|-----
  2   | DEHUMDIFICATOR
  3   | COLD
  4   | HOT
  6   | FAN
  0-1-7   | AUTO

#### Temp
param name : **stemp**

description: represents the target temperature

general accepted range 10-41

mode  | accepted range
:----:|---------------
AUTO  | 18-31
HOT   | 10-31
COLD  | 18-33


device memorize last target temp state for each mode under dft* (dft1,dft2...) parameters. You can't set directly these.

#### Fan rate
param name : **f_rate**

description: represents the fan rate mode

Daikin Emura FVXS does not support silence (=B) value for fan rate mode functionality.

value | desc
:----:|-----
A     | auto
B     | silence
3     | lvl_1
4     | lvl_2
5     | lvl_3
6     | lvl_4
7     | lvl_5

device memorize last fan rate state for each mode under dfr* (dfr1,dfr2...) parameters. You can't set directly these.

#### Fan direction
param name : **f_dir**

description: represents the fan direction

value | desc
:----:|-----
0     | all wings stopped
1     | vertical wings motion
2     | horizontal wings motion
3     | vertical and horizontal wings motion

device memorize last fan rate state for each mode under dfd* (dfd1,dfd2...) parameters. You can't set directly these.

#### Humidity
param name : **shum**

description: represents the target humidity

Daikin Emura FTXG-L and FVXS does not support humidity related functionality.

device memorize last humidity state for each mode under dh* (dh1,dh2...) parameters. You can't set directly these.

------------------------------
### Minimal parameters set

The `/aircon/set_control_info` has some mandatory parameters, it means that you need to include them in all the requests to this endpoint even if you are not interested in changing those specific values.

These are the mandatory parameters:
- `pow`
- `mode`
- `stemp`
- `shum`
- `f_rate`
- `f_dir`

While these are the ones that can be omitted:
- `adv`
- `dt*`
- `dh*`
- `dfr*`
- `dfd*`
- `b_mode`
- `b_stemp`
- `b_shum`
- `b_f_rate`
- `b_f_dir`
- `alert`

minimal request example: `pow=1&mode=1&stemp=26&shum=0&f_rate=B&f_dir=3`

### `/common/set_led`
#### Led
It seems that this settings doesn't actually change led.

param name: **led**

value | desc
:----:|-----
  -	| set to '0' (same as '0' value)
  0   | ?
  1   | ?


## Unsupported settings
This list show which hardware functionality are not supported by API

- led brightness switch


## Libraries
- [daikin-aricon-pylib](https://github.com/ael-code/daikin-aricon-pylib): Python library for managing Daikin air conditioners
- [jdaikin](https://bitbucket.org/JonathanGiles/jdaikin): Java-based API to interact with Daikin units


## Useful resource
- http://daikinsmartdbt.jp/ the site has been shut down but you can still have a look at the [cached page](https://github.com/ael-code/daikin-control/blob/readme_plus/daikinsmartdbt.htm)


## Control Info Examples

Switched Off
```
ret=OK,pow=0,mode=7,adv=,stemp=24.0,shum=0,dt1=24.0,dt2=M,dt3=25.0,dt4=25.0,dt5=25.0,dt7=24.0,dh1=0,dh2=50,dh3=0,dh4=0,dh5=0,dh7=0,dhh=50,b_mode=7,b_stemp=24.0,b_shum=0,alert=255,f_rate=4,f_dir=0,b_f_rate=4,b_f_dir=0,dfr1=4,dfr2=5,dfr3=7,dfr4=5,dfr5=5,dfr6=5,dfr7=4,dfrh=5,dfd1=0,dfd2=0,dfd3=3,dfd4=0,dfd5=0,dfd6=0,dfd7=0,dfdh=0
```
Auto 25C ( CONFORT AIR ) ( INTELLIGENT EYE )
```
ret=OK,pow=1,mode=7,adv=,stemp=25.0,shum=0,dt1=25.0,dt2=M,dt3=22.0,dt4=25.0,dt5=25.0,dt7=25.0,dh1=0,dh2=50,dh3=0,dh4=0,dh5=0,dh7=0,dhh=50,b_mode=7,b_stemp=25.0,b_shum=0,alert=255,f_rate=A,f_dir=0,b_f_rate=4,b_f_dir=0,dfr1=4,dfr2=5,dfr3=4,dfr4=5,dfr5=5,dfr6=5,dfr7=4,dfrh=5,dfd1=0,dfd2=0,dfd3=0,dfd4=0,dfd5=0,dfd6=0,dfd7=0,dfdh=0
```
```
ret=OK,pow=1&dh2=50&dfd4=0&b_stemp=25.0&alert=255&f_dir=0&b_shum=0&dh4=0&dfd3=0&dh3=0&dfd2=0&dfr2=5&dfr7=B&dfr4=5&dfd7=0&dfrh=5&dt3=25.0&dfdh=0&adv=&dh5=0&dh1=0&dfr6=5&dt5=25.0&dfr1=B&stemp=25.0&shum=0&dfd6=0&f_rate=A&b_f_dir=0&dt1=25.0&dhh=50&dfd1=0&dfr3=5&dh7=0&mode=1&dfd5=0&b_mode=7&dt4=25.0&b_f_rate=A&dt7=25.0&dt2=M&dfr5=5
```
Hot 25c ( AIR silence )
```
ret=OK,pow=1,mode=4,adv=,stemp=25.0,shum=0,dt1=25.0,dt2=M,dt3=22.0,dt4=25.0,dt5=25.0,dt7=25.0,dh1=0,dh2=50,dh3=0,dh4=0,dh5=0,dh7=0,dhh=50,b_mode=4,b_stemp=25.0,b_shum=0,alert=255,f_rate=B,f_dir=0,b_f_rate=B,b_f_dir=0,dfr1=B,dfr2=B,dfr3=B,dfr4=B,dfr5=B,dfr6=B,dfr7=B,dfrh=5,dfd1=0,dfd2=0,dfd3=0,dfd4=0,dfd5=0,dfd6=0,dfd7=0,dfdh=0
```

## More commands discovered

/common/get_datetime

the meaning is obvious and the response I got was:
ret=OK,sta=2,cur=2016/7/7 21:23:28,reg=eu,dst=1,zone=313

/aircon/get_scdltimer_info

it seems to get informations about the tasks scheduled on the AC, I have no further informations on that at the moment, but the response I got without anything scheduled was:
ret=OK,format=v1,f_detail=total#18;_en#1;_pow#1;_mode#1;_temp#4;_time#4;_vol#1;_dir#1;_humi#3;_spmd#2,scdl_num=3,scdl_per_day=6,en_scdltimer=1,active_no=1,scdl1_name=,scdl2_name=,scdl3_name=

/aircon/get_scdltimer_body&target=1

this seems to get detailed informations about a specific scheduled task, and the response I got was:
ret=OK,format=v1,target=1,en_scdltimer=1,moc=0,tuc=0,wec=0,thc=0,frc=0,sac=0,suc=0

/aircon/get_day_power_ex?days=2

this command retrieves the power consumption (in 0.1 kWH) of the last 2 days, separated hour by hour starting from midnight and also separated for cooling and heating. The days variable can be set up to 7 for last 7 days. The response I got was:
ret=OK,curr_day_heat=0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0,prev_1day_heat=0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0/0,curr_day_cool=0/0/0/0/0/0/0/0/0/0/0/0/1/0/0/0/0/2/2/2/2/0/0/0,prev_1day_cool=0/0/0/0/0/0/0/0/0/0/0/0/0/1/0/0/0/2/2/2/1/2/1/0
/aircon/get_week_power_ex
this command retrieves the power consumption (in 0.1 kWh) of the current and last week, separated day by day, the response I got was:
ret=OK,s_dayw=4,week_heat=0/0/0/0/0/0/0/0/0/0/0/0/0/0,week_cool=10/11/3/9/10/16/11/3/9/6/6/0/0/0
/aircon/get_year_power_ex
this command retrieves the power consumption (in 0.1 kWh) of the current and last year, separated month by month, the response I got was:
ret=OK,curr_year_heat=0/0/0/0/0/0/0/0/0/0/0/0,prev_year_heat=0/0/0/0/0/0/0/0/0/0/0/0,curr_year_cool=0/0/0/0/0/24/70/0/0/0/0/0,prev_year_cool=0/0/0/0/0/0/0/0/0/0/0/0
I found other requests to set the operating mode of the AC (the application uses GET HTTP requests):
/aircon/set_special_mode?en_streamer=0
this command disables the ion streamer on the AC (or it enables substituting ?en_streamer=1). The response has the form:
ret=OK,adv=13
or
ret=OK,adv=
where '13' means that the streamer has been activated or '' means that it has been deactivated
/aircon/set_special_mode?set_spmode=1&spmode_kind=1
this command enables the power mode of the AC and this can be seen looking at the adv=2 return message (even in the get_control_info)
/aircon/set_special_mode?set_spmode=1&spmode_kind=2
this command enables the econo mode of the AV and this can bee seen looking at the adv=12 return message (even in the get_control_info)
/aircon/set_special_mode?set_spmode=0&spmode_kind=1
this command disables the power mode of the AC (I think it also works without the spmode_kind variable and also that it also disable the econo mode, but I haven't checked so far)
