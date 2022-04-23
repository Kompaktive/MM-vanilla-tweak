# Motorsport Manager Vanilla Tweak
Motorsport Manager Vanilla Tweak
## List of Modified Classes
<details>
  <summary>CarPartStats</summary>
  
  * #### weightStrippedReliabilityMin
    Affects AI Team's weight stripping behavior relating to minimum reliability. In vanilla, the value is <code>0.5f</code> which means AI Team will weight strip their car parts down to 50% reliability, which is a very risky move. This mod changes it to <code>0.7f</code> to make sure they strip their car parts down to 70% only.
    ###### DEFAULT VALUE
    ```c#
    public const float weightStrippedReliabilityMin = 0.5f;
    ```
    ###### VANILLA TWEAK
    ```c#
    public const float weightStrippedReliabilityMin = 0.7f;
    ```
</details>

<details>
  <summary>PathData</summary>
  
  * #### CalculateLockUpZones()
    Affects how long in meters(?) the straight path is to enable the lock up zones. The default value is <code>400f</code> so if the straight path is less than 400 meters(?) then the lock up zones won't generate which means no driver will lock up in that spot. This mod changes it to <code>200f</code> so more lock up zones can be generated for each end of the straight path.
    ###### (?) = need confirmation
    ###### DEFAULT VALUE
    ```c#
    float num = 400f;
    ```
    ###### VANILLA TWEAK
    ```c#
    float num = 200f;
    ```
</details>

<details>
  <summary>TyreLockUpDirector</summary>
  
  * #### IsTyreLockUpViable()
    Affects when tyre lock up can trigger to each vehicle. For tyre lock up to happen, the function must return <code>true</code> .
    ###### DEFAULT VALUE
    ```c#
    bool isTutorialActiveInCurrentGameState = Game.instance.tutorialSystem.isTutorialActiveInCurrentGameState;
    bool flag = Game.instance.sessionManager.flag == SessionManager.Flag.Chequered;
    return inVehicle.speed <= GameUtility.MilesPerHourToMetersPerSecond(50f) && !isTutorialActiveInCurrentGameState && !inVehicle.behaviourManager.isOutOfRace && !flag && inVehicle.sessionEvents.IsReadyTo(SessionEvents.EventType.LockUp);
    ```
    ###### VANILLA TWEAK
    ```c#
    float focusSkill = 20f - inVehicle.driver.GetDriverStats().focus;
    float brakingSkill = 20f - inVehicle.driver.GetDriverStats().braking;
    float fitnessSkill = (Game.instance.sessionManager.GetNormalizedSessionTime() * 20f) - inVehicle.driver.GetDriverStats().fitness;
    float corneringSkill = 40f - inVehicle.driver.GetDriverStats().cornering - (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() * 20f);
    float adaptabilitySkill = (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() * 20f) - inVehicle.driver.GetDriverStats().adaptability;

    float minSpeedToTriggerLockUp = (inVehicle.driver.GetDriverStats().braking * 0.75f) + (inVehicle.driver.GetDriverStats().cornering * 0.75f) + (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() * 10f) - (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() * 10f);
    float lockUpChanceThreshold = (focusSkill + brakingSkill + fitnessSkill + adaptabilitySkill + corneringSkill) / 2000f;
    // change the 1000f to something else. The greater the value the less tyre lock up chance to trigger.

    bool isTutorialActiveInCurrentGameState = Game.instance.tutorialSystem.isTutorialActiveInCurrentGameState;
    bool flag = Game.instance.sessionManager.flag == SessionManager.Flag.Chequered;
    bool isLockUpTriggered = RandomUtility.GetRandom01() < lockUpChanceThreshold;
  
    return inVehicle.speed >= GameUtility.MilesPerHourToMetersPerSecond(minSpeedToTriggerLockUp) && !isTutorialActiveInCurrentGameState && !inVehicle.behaviourManager.isOutOfRace && !flag && isLockUpTriggered;
    ```
</details>

<details>
  <summary>RunningWideDirector</summary>
  
  * #### OnSessionStarting()
    Affects -
    ###### DEFAULT VALUE
    ```c#
    int k = RandomUtility.GetRandom(0, 2);
    ```
    ###### VANILLA TWEAK
    ```c#
    int k = RandomUtility.GetRandom(-3, 5);
    if (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() > 0.3f)
      k = RandomUtility.GetRandom(0, 5);
    ```
  * #### CanRunWide()
    Determines whether the subject is valid to run wide given it checks all the conditions.
    ###### DEFAULT VALUE
    ```c#
    if (RandomUtility.GetRandom01() < (float)this.mRunWidePathUseCount[inPath.pathID] / 3f)
    {
      return false;
    }
    bool flag = inVehicle.setup.tyreSet.GetTread() != SessionStrategy.GetRecommendedTreadRightNow() && RandomUtility.GetRandom01() < 0.1f;
    if (this.mActiveRunWideChunk.runWideCount <= 0 || this.mCooldown >= 0f)
    {
      return false;
    }
    if (Game.instance.sessionManager.eventDetails.currentSession.sessionType == SessionDetails.SessionType.Qualifying)
    {
      return flag;
    }
    float t = inVehicle.driver.GetDriverStats().focus / 20f;
    float num = Mathf.Lerp(1.5f, 2f, t);
    SessionWeatherDetails currentSessionWeather = Game.instance.sessionManager.currentSessionWeather;
    bool flag2 = Game.instance.sessionManager.flag == SessionManager.Flag.Green;
    bool flag3 = currentSessionWeather.GetNormalizedTrackWater() > 0.5f && currentSessionWeather.GetNormalizedRain() > 0.2f;
    bool flag4 = inVehicle.timer.gapToAhead > 0.1f && inVehicle.timer.gapToAhead < num;
    bool flag5 = inVehicle.timer.gapToBehind > 0.1f && inVehicle.timer.gapToBehind < num;
    bool flag6 = (flag5 && flag4) || flag4;
    return flag2 && (flag6 || flag || flag3);
    ```
    ###### VANILLA TWEAK
    ```c#
    if (this.mCooldown >= 0) return false;
  
    SessionWeatherDetails currentSessionWeather = Game.instance.sessionManager.currentSessionWeather;
    bool flag = inVehicle.setup.tyreSet.GetTread() != SessionStrategy.GetRecommendedTreadRightNow() && RandomUtility.GetRandom01() < 0.1f;
    bool flag2 = Game.instance.sessionManager.flag == SessionManager.Flag.Green;

    float focusSkill = 20f - inVehicle.driver.GetDriverStats().focus;
    float corneringSkill = 40f - inVehicle.driver.GetDriverStats().cornering - (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackRubber() * 20f);
    float brakingSkill = 20f - inVehicle.driver.GetDriverStats().braking;
    float fitnessSkill = (Game.instance.sessionManager.GetNormalizedSessionTime() * 20f) - inVehicle.driver.GetDriverStats().fitness;
    float adaptabilitySkill = (Game.instance.sessionManager.currentSessionWeather.GetNormalizedTrackWater() * 20f) - inVehicle.driver.GetDriverStats().adaptability;
    float pressureFromAhead = 20f - (inVehicle.timer.gapToAhead * 10f);
    float pressureFromBehind = 20f - (inVehicle.timer.gapToBehind * 10f);
    if (inVehicle.timer.gapToAhead > 2f || Game.instance.sessionManager.eventDetails.currentSession.sessionType != SessionDetails.SessionType.Race)
      pressureFromAhead = 0f;
    if (inVehicle.timer.gapToBehind > 2f || Game.instance.sessionManager.eventDetails.currentSession.sessionType != SessionDetails.SessionType.Race)
      pressureFromBehind = 0f;
    float runningWideChanceThreshold = (focusSkill + corneringSkill + brakingSkill + fitnessSkill + adaptabilitySkill + pressureFromAhead + pressureFromBehind) / 50000f;

    bool isRunningWideTriggered = RandomUtility.GetRandom01() < runningWideChanceThreshold;

    return flag2 && (flag || isRunningWideTriggered);
    ```
  * #### VehicleSetBehaviour()
    Affects -
    ###### DEFAULT VALUE
    ```c#
    this.mCooldown = 60f;
    this.mRunWidePathUseCount[inPath.pathID]++;
    ```
    ###### VANILLA TWEAK
    ```c#
    float focusSkill = inVehicle.driver.GetDriverStats().focus;
    this.mCooldown = focusSkill * 3f;
    this.mRunWidePathUseCount[inPath.pathID]++;
    ```
</details>
