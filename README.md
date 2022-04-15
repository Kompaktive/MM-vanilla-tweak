# Motorsport Manager Vanilla Tweak
Motorsport Manager Vanilla Tweak
## List of Modified Classes
<details>
  <summary><h3>CarPartStats</h3></summary>
  
  * #### weightStrippedReliabilityMin
    Affects AI Team's weight stripping behavior relating to minimum reliability. In vanilla, the value is <code>0.5f</code> which means AI Team will weight strip their car parts down to 50% reliability, which is a very risky move. This mod changes it to <code>0.7f</code> to make sure they strip their car parts down to 70% only.
    ##### DEFAULT VALUE
    ```c#
    public const float weightStrippedReliabilityMin = 0.5f;
    ```
    ##### VANILLA TWEAK
    ```c#
    public const float weightStrippedReliabilityMin = 0.7f;
    ```
</details>

<details>
  <summary><h3>PathData</h3></summary>
  
  * #### CalculateLockUpZones()
    Affects how long in meters(?) the straight path is to enable the lock up zones. The default value is <code>400f</code> so if the straight path is less than 400 meters(?) then the lock up zones won't generate which means no driver will lock up in that spot. This mod changes it to <code>200f</code> so more lock up zones can be generated for each end of the straight path.
    ###### (?) = need confirmation
    ##### DEFAULT VALUE
    ```c#
    float num = 400f;
    ```
    ##### VANILLA TWEAK
    ```c#
    float num = 200f;
    ```
</details>
