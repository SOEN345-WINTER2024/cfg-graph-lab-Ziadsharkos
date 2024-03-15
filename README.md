# Soot Tutorial
[![Build Status](https://travis-ci.com/noidsirius/SootTutorial.svg?branch=master)](https://travis-ci.com/noidsirius/SootTutorial)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/noidsirius/SootTutorial)
[![Docker Pull](https://img.shields.io/docker/pulls/noidsirius/soot_tutorial)](https://hub.docker.com/r/noidsirius/soot_tutorial)


This repository contains (will contain) several simple examples of static program analysis in Java using [Soot](https://github.com/Sable/soot).

## Who this tutorial is for?
Anybody who knows Java programming and wants to do some static analysis in practice but does not know anything about Soot and static analysis in theory.

If you have some prior knowledge about static program analysis I suggest you learn Soot from [here](https://github.com/Sable/soot/wiki/Tutorials).

### [Why another tutorial for Soot?](docs/Other/Motivation.md)

## Setup
In short, use Java 8 and run `./gradlew build`. For more information and Docker setup, follow this [link](docs/Setup/). 

## Chapters
### 1: Get your hands dirty

In this chapter, you will visit a very simple code example to be familiar with Soot essential data structures and **Jimple**, Soot's principle intermediate representation.

* `./gradlew run --args="HelloSoot"`: The Jimple representation of the [printFizzBuzz](demo/HelloSoot/FizzBuzz.java) method alongside the branch statement.
* `./gradlew run --args="HelloSoot draw"`: The visualization of the [printFizzBuzz](demo/HelloSoot/FizzBuzz.java) control-flow graph.



|Title |Tutorial | Soot Code        | Example Input  |
| :---: |:-------------: |:-------------:| :-----:|
|Hello Soot |[Doc](docs/1/)      | [HelloSoot.java](src/main/java/dev/navids/soottutorial/hellosoot/HelloSoot.java) | [FizzBuzz.java](demo/HelloSoot/FizzBuzz.java) |

<img src="docs/1/images/cfg.png" alt="Control Flow Graph" width="400"/>

### 2: Know the basic APIs

In this chapter, you get familiar with some basic but useful methods in Soot to help read, analyze, and even update java code.

* `./gradlew run --args="BasicAPI"`: Analyze the class [Circle](demo/BasicAPI/Circle.java).
* `./gradlew run --args="BasicAPI draw"`: Analyze the class [Circle](demo/BasicAPI/Circle.java) and draws the call graph.

|Title |Tutorial | Soot Code        | Example Input  |
| :---: |:-------------: |:-------------:| :-----:|
|Basic API |[Doc](https://medium.com/@noidsirius/know-the-basic-tools-in-soot-18f394318a9c)| [BasicAPI.java](src/main/java/dev/navids/soottutorial/basicapi/BasicAPI.java) | [Circle](demo/BasicAPI/Circle.java) |


<img src="docs/2/images/callgraph.png" alt="Call Graph" width="400"/>

### 3: Android Instrumentation

In this chapter, you learn how to insert code into Android apps (without having their source code) using Soot. To run the code, you need Android SDK (check this [link](docs/Setup/)).

* `./gradlew run --args="AndroidLogger"`: Insert logging method calls at the beginning of APK methods of [Numix Calculator](demo/Android/calc.apk).
* `./gradlew run --args="AndroidClassInjector"`: Create a new class from scratch and inject it to the  [Numix Calculator](demo/Android/calc.apk).

The instrumented APK is located in `demo/Android/Instrumented`. You need to sign it in order to install on an Android device:
```sh
cd ./demo/Android
# on Windows, replace 'sign.sh' by 'sign.ps1'
./sign.sh Instrumented/calc.apk key "android"
adb install -r -t Instrumented/calc.apk
```
To see the logs, run `adb logcat | grep -e "<SOOT_TUTORIAL>"`

|Title |Tutorial | Soot Code        | Example APK|
| :---: |:-------------: |:-------------:| :-----:|
|Log method calls in an APK| [Doc](https://medium.com/@noidsirius/instrumenting-android-apps-with-soot-dd6f146ff4d2)| [AndroidLogger.java](src/main/java/dev/navids/soottutorial/android/AndroidLogger.java) | [Numix Calculator](demo/Android/calc.apk) (from [F-Droid](https://f-droid.org/en/packages/com.numix.calculator/))|
|Create and inject a class into an APK| [Doc](https://medium.com/@noidsirius/instrumenting-android-apps-with-soot-dd6f146ff4d2) | [AndroidClassInjector.java](src/main/java/dev/navids/soottutorial/android/AndroidClassInjector.java) | [Numix Calculator](demo/Android/calc.apk) (from [F-Droid](https://f-droid.org/en/packages/com.numix.calculator/))|

<img src="docs/3/images/packs.png" alt="Soot Packs + Dexpler" width="400"/>

### 4: Call graphs and PointsTo Analysis in Android

This chapter gives you a brief overview o call graphs and PointsTo analysis in Android and you learn how to create calls graphs using FlowDroid. The source code of the example code is [here](demo/Android/STDemoApp). To run the code, you need Android SDK (check this [link](docs/Setup/)).

* `./gradlew run --args="AndroidCallGraph <CG_Algorithm> (draw)"`: Create the call graph of [SootTutorial Demo App](demo/Android/st_demo.apk) using `<CG_Algorithm>` algorithm and print information such as reachable methods or the number of edges.
    * `<CG_Algorithm>` can be `SPARK` or `CHA`
    * `draw` argument is optional, if provided a visualization of call graph will shown.
    * For example, `./gradlew run --args="AndroidCallGraph SPARK draw"` visualizes the call graph generated by SPARK algorithm.
* `./gradlew run --args="AndroidPTA"`: Perform PointsTo and Alias Analysis on [SootTutorial Demo App](demo/Android/st_demo.apk) using FlowDroid.

|Title |Tutorial | Soot Code        | Example APK|
| :---: |:-------------: |:-------------:| :-----:|
|Call graphs in Android| [Doc](https://medium.com/geekculture/generating-call-graphs-in-android-using-flowdroid-pointsto-analysis-7b2e296e6697)| [AndroidCallgraph.java](src/main/java/dev/navids/soottutorial/android/AndroidCallgraph.java) | [SootTutorial Demo App](demo/Android/st_demo.apk) ([source code](demo/Android/STDemoApp))|
|PointsTo Analysis in Android| [Doc](https://medium.com/geekculture/generating-call-graphs-in-android-using-flowdroid-pointsto-analysis-7b2e296e6697)| [AndroidPointsToAnalysis.java](src/main/java/dev/navids/soottutorial/android/AndroidPointsToAnalysis.java) | [SootTutorial Demo App](demo/Android/st_demo.apk) ([source code](demo/Android/STDemoApp))|

<img src="docs/4/images/Spark_CG.png" alt="The call graph of SootTutorial Demo app" width="400"/>



### 5: Some *Real* Static Analysis (:construction: WIP)

* `./gradlew run --args="UsageFinder 'void println(java.lang.String)' 'java.io.PrintStream"`: Find usages of the method with the given subsignature in all methods of [UsageExample.java](demo/IntraAnalysis/UsageExample.java).
* `./gradlew run --args="UsageFinder 'void println(java.lang.String)' 'java.io.PrintStream"`: Find usages of the method with the given subsignature of the given class signature in all methods of [UsageExample.java](demo/IntraAnalysis/UsageExample.java).


|Title |Tutorial | Soot Code        | Example Input  |
| :---: |:-------------: |:-------------:| :-----:|
|Find usages of a method| | [UsageFinder.java](src/main/java/dev/navids/soottutorial/intraanalysis/usagefinder/UsageFinder.java) | [UsageExample.java](demo/IntraAnalysis/usagefinder/UsageExample.java) |
|Null Pointer Analysis ||[NullPointerAnalysis](src/main/java/dev/navids/soottutorial/intraanalysis/npanalysis/) | [NullPointerExample.java](demo/IntraAnalysis/NullPointerExample.java) |

### 6: Interprocedural analysis (:construction: WIP)
|Title |Tutorial | Soot Code        | Example Input  |
| :---: |:-------------: |:-------------:| :-----:|
| | | | |

1. CFG:
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/9c3a6be6-dcbb-4def-a3c6-39175e4e8faf)

2. Node Coverage:
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/4bbfdf27-d483-4653-905c-6b9687aaa54d)

3. Edge Coverage:
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/600dff34-563f-40ba-b3bb-a37d7506b039)

4. Edge-Pair Coverage:
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/c94abe1f-fc01-44b6-8ad8-9865c3f89a09)

5. EFG: 
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/b5859fca-4aed-4bcf-83aa-a1656af2ae58)

# Part 2: The answers for Step 1-5 for the selected app for class project: Lexica 
Project function chosen:

 @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ThemeManager.getInstance().applyTheme(this);

        setContentView(R.layout.game);

        gameWrapper = findViewById(R.id.game_wrapper);
        toolbar = findViewById(R.id.toolbar);
        toolbar.setTitle(R.string.app_name);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        toolbar.setNavigationOnClickListener(v -> navigateToHome());

        if (savedInstanceState != null) {
            try {
                restoreGame(savedInstanceState);
            } catch (Exception e) {
                // On API < 11, the above should work fine because onSaveInstanceState should be
                // called before onPause. However, on API >= 11, onPause is always called _before_
                // onSaveInstanceState. In these cases, we will have to resort to the preferences
                // in order to restore our game (http://stackoverflow.com/a/28549669).
                Log.e(TAG, "error restoring state from savedInstanceState, trying to look for saved game in preferences", e);
                if (hasSavedGame()) {
                    if (!restoreGame()) {
                        finish();
                        return;
                    }
                }
            }
            return;
        }
        try {
            String action = getIntent().getAction();
            switch (action) {
                case "com.serwylo.lexica.action.RESTORE_GAME":
                    if (!restoreGame()) {
                        finish();
                        return;
                    }
                    break;

                case "com.serwylo.lexica.action.NEW_GAME":
                    GameMode gameMode = getIntent().getExtras().getParcelable("gameMode");
                    String[] board = getIntent().getExtras().getStringArray("board");

                    String langName = getIntent().getExtras().getString("lang");
                    Language language = Language.from(langName);

                    game = board != null
                        ? new Game(this, gameMode, language, board)
                        : Game.generateGame(this, gameMode, language);

                    setupGameView(game);
                    break;
            }
        } catch (Exception e) {
            Log.e(TAG, "Error creating new Lexica game. Will finish() the GameActivity in the hope it gracefully returns to the main menu.", e);
            finish();
        }
    }

## CFG Graph Drawn for chosen function from project(using Draw.io)

![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/86159c23-5c27-4013-80d9-d386a32dd369)

## Node Coverage for CFG (TRs and Test Paths):
TR: Covered Nodes: All nodes labeled with their corresponding code lines (1 to 28)

Test Paths:
Path 1: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13
Path 2: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Path 3: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Path 4: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Path 5: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13 -> 14 -> 15
Path 6: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Path 7: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 19
Path 8: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 20

## Edge Coverage for CFG (TRs and Test Paths):
Test Requirements:
Ensure that every edge in the CFG is traversed at least once.

Test Paths:
Execute the method with savedInstanceState as null.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13
Execute the method with a non-null savedInstanceState.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method and enter the catch block.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Execute the method and return from the catch block.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method and enter the if block for hasSavedGame().
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13 -> 14 -> 15
Execute the method and return from the if block for hasSavedGame().
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Execute the method and enter the switch block with action "com.serwylo.lexica.action.RESTORE_GAME".
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 19
Execute the method and enter the switch block with action "com.serwylo.lexica.action.NEW_GAME".
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 20

## Edge-Pair Coverage for CFG (TRs and Test Paths):
Test Requirements:
Ensure that every pair of edges in the CFG is traversed at least once.
Test Paths:
Execute the method with savedInstanceState as null.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method with a non-null savedInstanceState.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method and enter the catch block.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method and return from the catch block.
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 12 -> 14
Execute the method and enter the if block for hasSavedGame().
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 13 -> 14 -> 15
Execute the method and return from the if block for hasSavedGame().
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 12 -> 14
Execute the method and enter the switch block with action "com.serwylo.lexica.action.RESTORE_GAME".
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 19
Execute the method and enter the switch block with action "com.serwylo.lexica.action.NEW_GAME".
Edge 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9 -> 10 -> 11 -> 14 -> 17 -> 18 -> 20

## EFG:
![image](https://github.com/SOEN345-WINTER2024/cfg-graph-lab-Ziadsharkos/assets/93850680/6985172b-8e14-409f-b30e-734626d4d4f6)
