# Motorsport Manager Vanilla Tweak
Motorsport Manager Vanilla Tweak
## List of Modified Classes
<details>
  <summary>CarPartStats</summary>
  
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
  <summary>PathData</summary>
  
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

<details>
  <summary>TyreLockUpDirector</summary>
  
  * #### IsTyreLockUpViable()
    Affects when tyre lock up can trigger to each vehicle. For tyre lock up to happen, the function must return <code>true</code> .
    ##### DEFAULT VALUE
    ```c#
    bool isTutorialActiveInCurrentGameState = Game.instance.tutorialSystem.isTutorialActiveInCurrentGameState;
    bool flag = Game.instance.sessionManager.flag == SessionManager.Flag.Chequered;
    return inVehicle.speed <= GameUtility.MilesPerHourToMetersPerSecond(50f) && !isTutorialActiveInCurrentGameState && !inVehicle.behaviourManager.isOutOfRace && !flag && inVehicle.sessionEvents.IsReadyTo(SessionEvents.EventType.LockUp);
    ```
    For tyre lock up to trigger, one must have vehicle speed up to 50 MpH (Above it and the lock up won't happen), is not in tutorial mode, is not retired (<code>isOutOfRace</code>), is not chequered flag, and must ready to lockup. The last one (<code>IsReadyTo</code>) is calculated based on 'stress' point that will incremented overtime. Once it exceed over 1000, then <code>IsReadyTo</code> will return <code>true</code>.
    ##### VANILLA TWEAK
    ```c#
    float brakingSkill = 20f - inVehicle.driver.GetDriverStats().braking;
    float fitnessSkill = (Game.instance.sessionManager.GetNormalizedSessionTime() * 20f) - inVehicle.driver.GetDriverStats().fitness;
    float corneringSkill = 40f - inVehicle.driver.GetDriverStats().cornering - (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() * 20f);
    float adaptabilitySkill = (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() * 20f) - inVehicle.driver.GetDriverStats().adaptability;

    float minSpeedToTriggerLockUp = (inVehicle.driver.GetDriverStats().braking * 0.75f) + (inVehicle.driver.GetDriverStats().cornering * 0.75f) + (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() * 10f) - (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() * 10f);
    float lockUpChanceThreshold = (brakingSkill + fitnessSkill + adaptabilitySkill + corneringSkill) / 1000f;
    // change the 1000f to something else. The greater the value the less tyre lock up chance to trigger.

    bool isTutorialActiveInCurrentGameState = Game.instance.tutorialSystem.isTutorialActiveInCurrentGameState;
    bool flag = Game.instance.sessionManager.flag == SessionManager.Flag.Chequered;
    bool isLockUpTriggered = RandomUtility.GetRandom01() < lockUpChanceThreshold;
  
    return inVehicle.speed >= GameUtility.MilesPerHourToMetersPerSecond(minSpeedToTriggerLockUp) && !isTutorialActiveInCurrentGameState && !inVehicle.behaviourManager.isOutOfRace && !flag && isLockUpTriggered;
    ```
    In Vanilla Tweak, the minimum vehicle speed to trigger tyre lock up is changed to be more dynamic, calculated based on driver's braking and cornering skills, track rubber level, and track water level. <code>isReadyTo</code> is removed as well and replaced with <code>isLockUpTriggered</code>. <code>isLockUpTriggered</code> is calculated by driver's skill in braking, fitness, cornering, and adaptability. You don't have to worry about the formula, yes it is unreadable I know, however if you want to change the tyre lock up frequency, just change the line mentioned above the commented line or you can do experiment by making it from scratch.
</details>
