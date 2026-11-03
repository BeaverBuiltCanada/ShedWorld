# BeaverBuilt Notes

## todays goals starts here.

## Left wall window

1. prove I can get window to polulate the model, with defaults pre set to fit the minimum size structure of 4x4 with 3 1/2 feet walls. A 2x4 header should be the default. I think 12" height and 12" off floor for defaults.

- Left wall RO2 window

```text name:
CHOOSER_02_j_TOGGLE***Wall_Y_Left_Has_A_RO2_Window***TrueOrFalse
```

Fusion expects 0 or 1

```text name:
  CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested
```

Fusion expects a Unit in inches.

I have eye candy:
it is likely too busy and may come down the road in later versions. Some of it is for fail safe triggers. For example, I have:

```text name:
chooser_02_l_Wndw_RghOpn_RO2__Limited_WndwWidth_EyeCandyOnly
```

```text expression:
VAULT_2Across_OPENING_Wndw_RO2_MinMax_Width_Clamped
```

```text name:
chooser_02_m_Wndw_RghOpn_RO2__Limited_WndwWidth_TooBigBy_EyeCandyOnly
```

````text expression:
VAULT_2Across_Sys_OPENING_Wndw_RO2_YDistance_MinAllowed - VAULT_2Across_Sys_OPENING_Wndw_RO2_YDistance_MaxAllowed

- I have a limiter set in Fusion. 12" minimum to 72" maximum.
- (inch units) name is:
  VAULT_2Across_OPENING_Wndw_RO2_MinMax_Width_Clamped
- ```text expression is:
  if(CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested < 12 in; 12 in; if(CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested > 72 in; 72 in; CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested))
````

I also have a failsafe trigger in my Fusion model:

```text (inch units) name:
CHOOSER_02_n_VAULT_OPENING_Wndw_RO2_Width_FailSafeTrigger
```

```text expression:
   if(VAULT_2Across_Sys_OPENING_Wndw_RO2_YDistance_MinAllowed > VAULT_2Across_Sys_OPENING_Wndw_RO2_YDistance_MaxAllowed; Sketch_Sys_GHOST_InTheMachine_Thickness; if(CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested < 12 in; 12 in; if(CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested > 72 in; 72 in; CHOOSER_02_k_Wndw_RghOpn_RO2__Limited_WindowWidth_Inches_Requested)))
```

and my Fusion model uses:

```text name:
CHOOSER_02_o_Wndw_RghOpn_RO2__Limited_Height_Inches_Requested
```

-Fusion expects an input in Unit inches.

```text name:
chooser_02_p_Wndw_RghOpn_RO2__Limited_Height_EyeCandy_Actual
```

```text expression:
VAULT_2Up_OPENING_Wndw_RO2_OAopnH_MinMax_Height_Clamped
```

```text name:
chooser_02_q_VAULT_2Up_OPENING_Wndw_RO2_CanItFit_FloorToTopOfHeader_EyeCandy
```

```text expression:
VAULT_2Up_OPENING_Wndw_RO2_ZDistance_DistFrmFlr_Clamped + Sketch_RO2_VAULT_2Up_OPENING_Wndw_OAopnH_Height_Final + VAULT_2Up_Sys_OPENING_Wndw_RO2_HeaderStack_Height_Resolver
```

The tricky part is, knowing what I need for Vue configurator and what is back ground system parameters and eye candy noise.

I think we should have another compute in Vues, similar to how the warning was for shed width being greater than shed depth.
For determining max window size allowed, the compute should do wall length, and likely subtract a bay on each end. (16"x2 if 2x4 and 24" x 2 if 2x6 studs) and throw a warning like, window is too wide for wall length.

another thing I thought would be cool for choosing window width and height size was if we could have a dragable square, similar to a cropping tool with numbers in a text box that could slide greater or lesser, auto populating beside the crop window... if that is doable.

The Fusion model is stable and will not implode if the size chosen will not fit the wall. I need a way to communicate that, especially because I will not have a rendered model for them, which really sucks!

CHOOSER_02_o_Wndw_RghOpn_RO2\_\_Limited_Height_Inches_Requested

CHOOSER_02_r_Wndw_RghOpn_RO2\_\_Limited_ZDistance_DistFrmFlr_Inches_Requested

CHOOSER_02_v_RO2_OPENING\_\_\_Place_Window_By_Stud_Indexer

CHOOSER_02_w_RO2_OPENING***WindowUse_2x6_Headers***TrueOrFalse

Lets get this working and see where we are.

- Header sizing
- Traveler card cleanup

## Future Ideas

- Blueprint unrolling into warning animation
- Navigation panel
- Drawing package matching
  Future Feature:
  Design Review Assistant

✅ Good Decisions
💡 Suggestions
⚠️ Potential Issues

copy paste place holder
what I currently have for window

```text
About This Configurator

The Beaver Built Canada Configurator is currently under active development.

Our long-term goal is to provide accurate 3D visualization and real-time rendering so you can see your building take shape as you make selections. At this stage, the configurator focuses on gathering design information and generating a build traveler that helps guide the drawing process.

Because every building is different, some design decisions still require human review. While the configurator captures your goals and preferences, a real person reviews each project and prepares the drawing package.

This allows us to:

Catch potential design conflicts
Improve structural layouts where appropriate
Make practical framing adjustments
Ensure the final design remains buildable and cost-effective

As an early user, your flexibility and feedback help shape future versions of the configurator.

or another version:

Beaver Built Canada is currently using a hybrid approach. The configurator gathers your design intent, but final drawings are still reviewed by a real human. This allows us to make practical framing decisions where necessary, catch unusual situations, and continue improving the system. Early customers are helping us build toward a future with accurate rendered models and more automated design tools.

```

```text
Left Window Reference Diagram
 For added clarity, this reference image illustrates a typical 8' wide × 12' deep × 8' wall height shed with the roof peak running in the depth direction. The left wall window rough opening shown is 42" wide × 32" high, with the rough opening positioned 36" above the floor. The window is located in Stud Bay 1. A Stud Index of 0 represents the default centered position in the wall.

This image is provided as a reference to help explain window configuration settings. Your actual building dimensions, window sizes, and locations may differ based on the selections made within the configurator.

The reference images throughout the configurator are also representative of the style and level of detail customers can expect to see in their final drawing package. Additional drawing examples and sample plans will be provided as the project continues to develop.
```

```text
Build a drawing package structure:
Page 1
--------
Cover Sheet

Page 2
--------
Foundation Plan

Page 3
--------
Floor Framing

Page 4
--------
Left Wall

Page 5
--------
Right Wall

Page 6
--------
Front Wall

Page 7
--------
Rear Wall

Page 8
--------
Roof Framing

Page 9
--------
Material List
Then every building uses that structure.

The dimensions change.

The layout does not.
```

```text
For window distance from front corner:
Fusion stud spacing over ride and what it expects from json
================================================================
this parameter turns stud spacing over ride on or off
Currently we have no inputs for this .
If this is set to 1, the inputs should fall to between 12 and 48" I would say likely step 2" seems reasonable.

Name: CHOOSER_01d_ssii_TOGGLE___WALLS_Stud_Spacing_Inches_ManualOverRide_TrueOrFalse
    unitless: 1 or 0

===============================================================
This is a inch unit, so Fusion expects a value such as 12, 18, 20 etc. I would include 16 and 24 in this for the simple reason, someone may want to use 2x4 on 24" centers. That is really the whole reason behind the over ride. Wierd numbers such as 32" are more for animal housing type structures. Or who knows, this could likely easily morph into cabinetry. Same principals. I shouldnt say easily though. oops. If it was easy, everyone would be doing it.

Name: CHOOSER_01d_ssiii___WALLS_Stud_SpacingForOverRide_INCHES
Comment: MinMax limiters 12 - 48
Uses: Name: VAULT_UNIVERSE_WALL_XY_Stud_Spacing_Clamped
    Expression: CHOOSER_01d_ssii_TOGGLE___WALLS_Stud_Spacing_Inches_ManualOverRide_TrueOrFalse * if(CHOOSER_01d_ssiii___WALLS_Stud_SpacingForOverRide_INCHES < 12 in; 12 in; if(CHOOSER_01d_ssiii___WALLS_Stud_SpacingForOverRide_INCHES > 48 in; 48 in; CHOOSER_01d_ssiii___WALLS_Stud_SpacingForOverRide_INCHES)) + ( 1 - CHOOSER_01d_ssii_TOGGLE___WALLS_Stud_Spacing_Inches_ManualOverRide_TrueOrFalse ) * ( CHOOSER_01d_ssi_TOGGLE____WALLS_Use_2x6_STUDS_TrueorFalse * 24 in + ( 1 - CHOOSER_01d_ssi_TOGGLE____WALLS_Use_2x6_STUDS_TrueorFalse ) * 16 in )


We are already using:
CHOOSER_02_v_RO2_OPENING___Place_Window_By_Stud_Indexer
CHOOSER_04_E___RO4_OPENING___Place_Window_By_Stud_Indexer
```

```text
for windows, I wrapped the window section in a class:

<!--=====START OF LEFT WALL WINDOW - at top header======-->
          <div class="window-section">  here
            <h2>Left Wall Window</h2>

            <p>
              <button
                @click="showWhatIsALeftWallWindow = !showWhatIsALeftWallWindow"
              >
                What is a left wall window?
              </button>
            </p>

            <p v-if="showWhatIsALeftWallWindow">
              <strong class="whatIs-q">Q: What is a left wall window?</strong
              ><br />
              A: A left wall window is...
            </p>

            <div class="current_selection" v-if="leftWallRO2Window === 1">
              Left Wall Window: Included
            </div>

and here I introduced the class Q-card idea...
            <div
              class="left-traveler-windowQ-card"
              :class="{ left_selected_window: leftWallRO2Window === 1 }"
              @click="leftWallRO2Window = leftWallRO2Window === 1 ? 0 : 1"
            >
              <h3>🪟 Add Left Wall Window</h3>
            </div>

```
