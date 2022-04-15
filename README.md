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
