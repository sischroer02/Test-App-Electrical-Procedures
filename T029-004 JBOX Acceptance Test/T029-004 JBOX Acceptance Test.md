# T029-004 JBOX Acceptance Test

``` yaml boring-test
machine: T029 - Electrical
testname: T029-004 JBOX Acceptance Test
classification: T029 - Electrical
```

## Purpose

(1) Electrically Test the Standard JBOX for Power Distribution and Control Signals

``` python borelib
import asyncio
import logging
from tbclib.symbols import BroccoliSymbolService, SymbolHandle, SymbolManager, get_target
from tbclib import RESULT

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Define Broccoli service URL
BROC_URL = "http://10.14.202.41:1358"

# Define Structure of Tests
Test_Library_480V = {
    1: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_4', 'TEST_NAME' : '480V Supplied S29 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Up', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Up'},
    # 2: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_5', 'TEST_NAME' : '480V Supplied S29 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Us', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Us'},
    # 3: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_4', 'TEST_NAME' : '480V Supplied S30 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Up'},
    2: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_5', 'TEST_NAME' : '480V Supplied S30 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Us'},
    3: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_1', 'TEST_NAME' : '480V Supplied S33 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Up'},
    4: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_2', 'TEST_NAME' : '480V Supplied S33 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Us'},
    5: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_1', 'TEST_NAME' : '480V Supplied S34 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Up'},
    6: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_2', 'TEST_NAME' : '480V Supplied S34 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Us'},
    7: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_1', 'TEST_NAME' : '480V Supplied S25 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Up'},
    8: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_2', 'TEST_NAME' : '480V Supplied S25 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Us'},
    9: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_1', 'TEST_NAME' : '480V Supplied S26 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Up'},
    10: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_2', 'TEST_NAME' : '480V Supplied S26 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Us'},
    11: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_1', 'TEST_NAME' : '480V Supplied S27 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Up'},
    12: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_2', 'TEST_NAME' : '480V Supplied S27 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Us'},
    13: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_1', 'TEST_NAME' : '480V Supplied S28 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Up'},
    14: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_2', 'TEST_NAME' : '480V Supplied S28 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Us'},
    15: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF1', 'TEST_NAME' : 'Mine Phone Continuity SF1 Check', 'Variable Name' : 'JBOX_GVL_IO.SF1_MF_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin1'},
    16: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF2', 'TEST_NAME' : 'Mine Phone Continuity SF2 Check', 'Variable Name' : 'JBOX_GVL_IO.SF2_MF_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin2'},
    17: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF4', 'TEST_NAME' : 'Fire Pull Station Pulled SF4 Check', 'Variable Name' : 'JBOX_GVL_IO.SF4_Pull_Station', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No continuity on AUX pin4 through pull station when it is pulled'},
    18: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF6', 'TEST_NAME' : 'MGT Continuity SF6 Check', 'Variable Name' : 'JBOX_GVL_IO.SF6_MGT_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin6'},
    19: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF8', 'TEST_NAME' : 'MGT Continuity SF8 Check', 'Variable Name' : 'JBOX_GVL_IO.SF8_MGT_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin8'},
    20: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX Door Switch Closed Check', 'Variable Name' : 'JBOX_GVL_IO.JB_Door_Switch', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - Did not Correctly Read Door Switch As Closed'},
    21: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX DCDCs Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_DCDCS_OK', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - DCDCs not outputting "OK" Signal'},
    22: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX PSUs Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSUs_OK', 'XV' : False, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - PSU "OK" Signal is True when it should be False'},
    23: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX 480V PSU2 Current Reading Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSU2_IOUT', 'XV' : 40, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - 480V PSU2 is not outputting valid amperage reading signal'}
    
}

Test_Library_120V = {
    1: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_4', 'TEST_NAME' : '120V Supplied S29 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Up', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Up'},
    # 2: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_5', 'TEST_NAME' : '120V Supplied S29 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Us', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Us'},
    # 3: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_4', 'TEST_NAME' : '120V Supplied S30 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Up'},
    2: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_5', 'TEST_NAME' : '120V Supplied S30 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Us'},
    3: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_1', 'TEST_NAME' : '120V Supplied S33 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Up'},
    4: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_2', 'TEST_NAME' : '120V Supplied S33 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Us'},
    5: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_1', 'TEST_NAME' : '120V Supplied S34 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Up'},
    6: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_2', 'TEST_NAME' : '120V Supplied S34 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Us'},
    7: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_1', 'TEST_NAME' : '120V Supplied S25 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Up'},
    8: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_2', 'TEST_NAME' : '120V Supplied S25 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Us'},
    9: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_1', 'TEST_NAME' : '120V Supplied S26 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Up'},
    10: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_2', 'TEST_NAME' : '120V Supplied S26 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Us'},
    11: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_1', 'TEST_NAME' : '120V Supplied S27 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Up'},
    12: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_2', 'TEST_NAME' : '120V Supplied S27 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Us'},
    13: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_1', 'TEST_NAME' : '120V Supplied S28 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Up'},
    14: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_2', 'TEST_NAME' : '120V Supplied S28 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Us'},
    15: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF4', 'TEST_NAME' : 'Fire Pull Station Nominal SF4 Check', 'Variable Name' : 'JBOX_GVL_IO.SF4_Pull_Station', 'XV' : False, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - Continuity on AUX Pin4 when the pull station is not pulled'},
    16: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX Door Switch Open Check', 'Variable Name' : 'JBOX_GVL_IO.JB_Door_Switch', 'XV' : False, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - Did not Correctly Read Door Switch As Open'},                                                 
    17: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX DCDCs Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_DCDCS_OK', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - DCDCs not outputting "OK" Signal'},
    18: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX PSUs Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSUs_OK', 'XV' : False, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - PSU "OK" Signal is True when it should be False'},
    20: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX 120V PSU1 Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSU1_IOUT', 'XV' : 40, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - 120V PSU1 is not outputting valid amperage reading signal'}
}

Test_Library_Redundant = {
    1: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_4', 'TEST_NAME' : '480V Supplied S29 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Up', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Up'},
    # 2: {'JBOX_PORT' : 'S29', 'TEST_LABEL' : 'J7_5', 'TEST_NAME' : '480V Supplied S29 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J7_24V_Us', 'XV' : True, 'Pass Value' : 'PASS', 'Fail Value' : 'FAIL - No 24V Available for JBOX S29 Us'},
    # 3: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_4', 'TEST_NAME' : '480V Supplied S30 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Up'},
    2: {'JBOX_PORT' : 'S30', 'TEST_LABEL' : 'J8_5', 'TEST_NAME' : '480V Supplied S30 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J8_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S30 Us'},
    3: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_1', 'TEST_NAME' : '480V Supplied S33 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Up'},
    4: {'JBOX_PORT' : 'S33', 'TEST_LABEL' : 'J1_2', 'TEST_NAME' : '480V Supplied S33 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J1_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S33 Us'},
    5: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_1', 'TEST_NAME' : '480V Supplied S34 Up Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Up'},
    6: {'JBOX_PORT' : 'S34', 'TEST_LABEL' : 'J2_2', 'TEST_NAME' : '480V Supplied S34 Us Lighting 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J2_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S34 Us'},
    7: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_1', 'TEST_NAME' : '480V Supplied S25 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Up'},
    8: {'JBOX_PORT' : 'S25', 'TEST_LABEL' : 'J3_2', 'TEST_NAME' : '480V Supplied S25 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J3_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S25 Us'},
    9: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_1', 'TEST_NAME' : '480V Supplied S26 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Up'},
    10: {'JBOX_PORT' : 'S26', 'TEST_LABEL' : 'J4_2', 'TEST_NAME' : '480V Supplied S26 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J4_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S26 Us'},
    11: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_1', 'TEST_NAME' : '480V Supplied S27 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Up'},
    12: {'JBOX_PORT' : 'S27', 'TEST_LABEL' : 'J5_2', 'TEST_NAME' : '480V Supplied S27 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J5_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S27 Us'},
    13: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_1', 'TEST_NAME' : '480V Supplied S28 Up 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Up', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Up'},
    14: {'JBOX_PORT' : 'S28', 'TEST_LABEL' : 'J6_2', 'TEST_NAME' : '480V Supplied S28 Us 24V Check', 'Variable Name' : 'JBOX_GVL_IO.J6_24V_Us', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No 24V Available for JBOX S28 Us'},
    15: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF1', 'TEST_NAME' : 'Mine Phone Continuity SF1 Check', 'Variable Name' : 'JBOX_GVL_IO.SF1_MF_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin1'},
    16: {'JBOX_PORT' : 'S2', 'TEST_LABEL' : 'SF2', 'TEST_NAME' : 'Mine Phone Continuity SF2 Check', 'Variable Name' : 'JBOX_GVL_IO.SF2_MF_CONTINUITY', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - No Continuity On AUX Pin2'},
    17: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX PSUs Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSUs_OK', 'XV' : True, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - PSU "OK" Is not True When Both PSUs are on, it should be'},
    18: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX 480V PSU2 Current Reading Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSU2_IOUT', 'XV' : 40, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - 480V PSU2 is not outputting valid amperage reading signal'},
    19: {'JBOX_PORT' : 'INTERNAL', 'TEST_LABEL' : 'INTERNAL', 'TEST_NAME' : 'JBOX 120V PSU1 Nominal Operation Check', 'Variable Name' : 'JBOX_GVL_IO.JB_PSU1_IOUT', 'XV' : 40, 'Pass Value' : "PASS", 'Fail Value' : 'FAIL - 120V PSU1 is not outputting valid amperage reading signal'}
}




class JboxTestStandBaseTest:
    def __init__(self):
        self.testvariable = True
        self.service = BroccoliSymbolService(broc_url=BROC_URL, logger=logger)
        self.NUM_TESTS_PASSED_480V = 0
        self.NUM_TESTS_PASSED_120V = 0
        self.NUM_TESTS_PASSED_REDUNDANT = 0


    async def Read_480V_Supplied_Test_IO(self):
        self.NUM_TESTS_PASSED_480V
        try:
            # Initialize the service
            await self.service.initialize()
            logger.info("Broccoli service initialized")

            # Read all symbols for 

            for key, entry in Test_Library_480V.items():         
                self.variable_name = entry['Variable Name']
                self.expected_value = entry['XV']
                self.current_symbol = SymbolHandle(self.variable_name)
                self.current_value = self.service.read_symbol(self.current_symbol)

                logger.info("Read symbol %s: %s", self.current_symbol.name, bool(self.current_value))

                if self.variable_name == 'JBOX_GVL_IO.JB_PSU2_IOUT':
                    if self.current_value >= 0 and self.current_value <= 10000:
                        RESULT.set_metadata(Test_Library_480V[key]["TEST_NAME"], Test_Library_480V[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_480V += 1
                    else:
                        RESULT.set_metadata(Test_Library_480V[key]["TEST_NAME"], Test_Library_480V[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")

                else:
                    if self.expected_value == self.current_value:
                        RESULT.set_metadata(Test_Library_480V[key]["TEST_NAME"], Test_Library_480V[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_480V += 1
                    else:
                        RESULT.set_metadata(Test_Library_480V[key]["TEST_NAME"], Test_Library_480V[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")

        except Exception as e:
            logger.error("Error reading symbols: %s", e)
            RESULT.set_metadata("Read Error", str(e))
            RESULT.save_delta()

        finally:
            logger.info(self.NUM_TESTS_PASSED_480V)
            RESULT.set_metadata('Number of 480V Supplied Tests Passed', self.NUM_TESTS_PASSED_480V)
            RESULT.save_delta()
            # Clean up
            try:
                await self.service.close()
                logger.info("Broccoli service closed")
            except Exception as e:
                logger.error("Error closing service: %s", e)

    async def Read_120V_Supplied_Test_IO(self):
        self.NUM_TESTS_PASSED_120V
        try:
            # Initialize the service
            await self.service.initialize()
            logger.info("Broccoli service initialized")

            # Read all symbols for 

            for key, entry in Test_Library_120V.items():
                self.variable_name = entry['Variable Name']
                self.expected_value = entry['XV']
                self.current_symbol = SymbolHandle(self.variable_name)
                self.current_value = self.service.read_symbol(self.current_symbol)

                logger.info("Read symbol %s: %s", self.current_symbol.name, bool(self.current_value))

                if self.variable_name == 'JBOX_GVL_IO.JB_PSU1_IOUT':
                    if self.current_value >= 0 and self.current_value <= 10000:
                        RESULT.set_metadata(Test_Library_120V[key]["TEST_NAME"], Test_Library_120V[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_120V += 1
                    else:
                        RESULT.set_metadata(Test_Library_120V[key]["TEST_NAME"], Test_Library_120V[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")
                        
                else:
                    if self.expected_value == self.current_value:
                        RESULT.set_metadata(Test_Library_120V[key]["TEST_NAME"], Test_Library_120V[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_120V += 1
                    else:
                        RESULT.set_metadata(Test_Library_120V[key]["TEST_NAME"], Test_Library_120V[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")


        except Exception as e:
            logger.error("Error reading symbols: %s", e)
            RESULT.set_metadata("Read Error", str(e))
            RESULT.save_delta()

        finally:
            logger.info(self.NUM_TESTS_PASSED_120V)
            RESULT.set_metadata('Number of 120V Supplied Tests Passed', self.NUM_TESTS_PASSED_120V)
            RESULT.save_delta()
            # Clean up
            try:
                await self.service.close()
                logger.info("Broccoli service closed")
            except Exception as e:
                logger.error("Error closing service: %s", e)

    async def Read_Redundant_Supplied_Test_IO(self):
        self.NUM_TESTS_PASSED_REDUNDANT
        try:
            # Initialize the service
            await self.service.initialize()
            logger.info("Broccoli service initialized")

            # Read all symbols for 

            for key, entry in Test_Library_Redundant.items():
                self.variable_name = entry['Variable Name']
                self.expected_value = entry['XV']
                self.current_symbol = SymbolHandle(self.variable_name)
                self.current_value = self.service.read_symbol(self.current_symbol)

                logger.info("Read symbol %s: %s", self.current_symbol.name, bool(self.current_value))

                if self.variable_name == 'JBOX_GVL_IO.JB_PSU1_IOUT' or self.variable_name == 'JBOX_GVL_IO.JB_PSU2_IOUT':
                    if self.current_value >= 0 and self.current_value <= 10000:
                        RESULT.set_metadata(Test_Library_Redundant[key]["TEST_NAME"], Test_Library_Redundant[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_REDUNDANT += 1
                    else:
                        RESULT.set_metadata(Test_Library_Redundant[key]["TEST_NAME"], Test_Library_Redundant[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")
                        
                else:
                    if self.expected_value == self.current_value:
                        RESULT.set_metadata(Test_Library_Redundant[key]["TEST_NAME"], Test_Library_Redundant[key]["Pass Value"])
                        RESULT.save_delta()
                        logger.info("Pass result saved")
                        self.NUM_TESTS_PASSED_REDUNDANT += 1
                    else:
                        RESULT.set_metadata(Test_Library_Redundant[key]["TEST_NAME"], Test_Library_Redundant[key]["Fail Value"])
                        RESULT.save_delta()
                        logger.info("Fail result saved")


        except Exception as e:
            logger.error("Error reading symbols: %s", e)
            RESULT.set_metadata("Read Error", str(e))
            RESULT.save_delta()

        finally:
            logger.info(self.NUM_TESTS_PASSED_REDUNDANT)
            RESULT.set_metadata('Number of Redundant Supplied Tests Passed', self.NUM_TESTS_PASSED_REDUNDANT)
            RESULT.save_delta()
            # Clean up
            try:
                await self.service.close()
                logger.info("Broccoli service closed")
            except Exception as e:
                logger.error("Error closing service: %s", e)

```

***

## Pre-Test

***

### 1\. Test Rules

(1.1) 1 operator is needed for this test

(1.2) Do not step away from test during the procedure

(1.3) Disconnect Power Supply Before making any connections, do not make live connections.

```yaml test-field
name: Rules Understood
type: boolean

validation: true
continue_on_fail: false
```

***

### 2\. Goal of JBOX Testing

(2.1) Validate 480V, 120V, and 24V Power Distribution within the panel and presence of voltage at all external connections.

(2.2) Validate primary JBOX control signals, including door switch, estop, and pull station.

```yaml test-field
name: Goals Understood
type: boolean

validation: true
continue_on_fail: false
```

***

## 480V Test Section Setup

***

### 1\. Hookup These Electrical Connections

| Test Enclosure Cable | JBOX Port |
|----------------------|-----------|
| SF1 | J37 |
| SF2 | S2 |
| J1 | S33 |
| J2 | S34 |
| J3 | S25 |
| J4 | S26 |
| J5 | S27 |
| J6 | S28 |
| J7 | S29 |
| J8 | S30 |
| J9 | S1 |
| J11 | S3 |
| J12 | S5 |
| J13 | S8 |
| J14 | S10 |
| J15 | S21 |
| J16 | S22 |
| J17 | S23 |
| J18 | S24 |
| J19 | S4 |
| J20 | S6 |
| J21 | S9 |
| J22 | S11 |
| J23 | S13 |
| J24 | S14 |
| J25 | S15 |
| J26 | S16 |
| S17 | J36 |

```yaml test-field
name: Electrical Connections all made
type: boolean

validation: true
continue_on_fail: false
```


***

### 2\. 480V PSU2 and DCDC Configuration

(2.1) Configure 480V PSU2 Following the Steps Below
- Open the Phoenix Contact "Quint Power" App
- Install temportary 22 AWG jumper wire from PSU2 Rem signal to 0V TB'
- Select READ VIA NFC
- Select SCAN VIA NFC
- Place top edge of phone over the QR code on PSU2 until power supply is read successfully
- Select OUTPUT VOLTAGE menu
- Set OUTPUT VOLTAGE to 26.00V
- Set PARALLEL OPERATION to TRUE
- Exit OUTPUT VOLTAGE menu and select SIGNALING menu
- Set OUT 2 to ANALOG OUTPUT with OUTPUT CURRENT
- Exit SIGNALING menu
- Select NFC button and plade top edge of phone over the QR code on PSU1 until the dara is written succesfully
- Remove temportary 22 AWG jumper wire FROM PSU2 Rem signal TO 0V TB
- Close Quint Power App
- Place "PSU CONFIGURED, todays DATE, your initials" Label on the PSU

```yaml test-field
name: JBOX 480V PSU2 Configured
type: boolean
validation: true
continue_on_fail: false
```

(2.2) Configure DCDC1 Following the Steps Below
- Pull ferrules inserted into terminals 2.1 and 2.2 in DC/DC slightly out so you can probe voltage on exposed ferrule
- Turn Uout dial on DC/DC until the DC/DCs output is 52V. Probe on exposed ferrules to verify
- Reseat ferrules to correct depth
- Put fuse 13 back
- Place "52V, todays DATE, your initials"  label on the DC/DC

```yaml test-field
name: JBOX DCDC1 Configured
type: boolean
validation: true
continue_on_fail: false
```

(2.3) Configure DCDC2 Following the Steps Below
- Pull ferrules inserted into terminals 2.1 and 2.2 in DC/DC slightly out so you can probe voltage on exposed ferrule
- Turn Uout dial on DC/DC until the DC/DCs output is 52V. Probe on exposed ferrules to verify
- Reseat ferrules to correct depth
- Put fuse 13 back
- Place "52V, todays DATE, your initials"  label on the DC/DC

```yaml test-field
name: JBOX DCDC2 Configured
type: boolean
validation: true
continue_on_fail: false
```

***

## Test Section 480V Supplied Power Distribution, AUX Continuity, and JBOX IO

***

### 1\. Initial conditions

(1.1) Power Cycle the JBOX By Turning OFF and ON the three breakers in the upper left of the jbox.

(1.2) Ensure Fire alarm pull station is pulled

(1.3) Ensure JBOX Door is Closed

```yaml test-field
name: Test Stand Conditions Confirmed
type: boolean
validation: true
continue_on_fail: false
```

***

### 2\. Execute 480V Supplied Automated Test

(2.1) run function set

``` python borescript FunctionSet tbclib
async def run_test():
    base_test = JboxTestStandBaseTest()
    await base_test.Read_480V_Supplied_Test_IO()


if __name__ == "__main__":
    asyncio.run(run_test())

```

``` yaml test-field
name: Number of 480V Supplied Tests Passed
type: number

validation:
    low: 23
    high: 23

required: true
```

***

## 120V Test Section Setup

***

### 1\. Electrical Connections

(1.1) Disconnect these Electrical Connections

| Test Enclosure Cable | JBOX Port |
|----------------------|-----------|
| UNSURE | J36 |
| UNSURE | S1 |

(1.2) Hookup These Electrical Connections

| Test Enclosure Cable | JBOX Port |
|----------------------|-----------|
| UNSURE | J10 |
| UNSURE | S18 |

```yaml test-field
name: Electrical Connections all made
type: boolean

validation: true
continue_on_fail: false
```

***

### 2\. 120V PSU1 Configuration

(2.1) Configure 120V PSU1 Following the Steps Below
- Open the Phoenix Contact "Quint Power" App
- Install temportary 22 AWG jumper wire from PSU1 Rem signal to 0V TB'
- Select READ VIA NFC
- Select SCAN VIA NFC
- Place top edge of phone over the QR code on PSU1 until power supply is read successfully
- Select OUTPUT VOLTAGE menu
- Set OUTPUT VOLTAGE to 26.00V
- Set PARALLEL OPERATION to TRUE
- Exit OUTPUT VOLTAGE menu and select SIGNALING menu
- Set OUT 2 to ANALOG OUTPUT with OUTPUT CURRENT
- Exit SIGNALING menu
- Select NFC button and plade top edge of phone over the QR code on PSU1 until the dara is written succesfully
- Remove temportary 22 AWG jumper wire FROM PSU1 Rem signal TO 0V TB
- Close Quint Power App
- Place "PSU CONFIGURED, todays DATE, your initials" Label on the PSU

```yaml test-field
name: JBOX 480V PSU1 Configured
type: boolean
validation: true
continue_on_fail: false
```

***

## Test Section 120V Supplied Power Distribution, AUX Continuity, and JBOX IO

***

### 1\. Initial conditions

(1.1) Power Cycle the JBOX By Turning OFF and ON the three breakers in the upper left of the jbox.

(1.2) Ensure Fire alarm pull station is not pulled, reset it if needed with the key

(1.3) Ensure JBOX Door is Open

```yaml test-field
name: Test Stand Conditions Confirmed
type: boolean
validation: true
continue_on_fail: false
```

***


### 2\. Execute 480V Supplied Automated Test


(2.1) run function set

``` python borescript FunctionSet tbclib
async def run_test():
    base_test = JboxTestStandBaseTest()
    await base_test.Read_120V_Supplied_Test_IO()

# TestFunctionSet tbclib

if __name__ == "__main__":
    asyncio.run(run_test())

```

```yaml test-field
name: Number of 120V Supplied Tests Passed
type: number

validation:
    low: 19
    high: 19

required: true
```

## Test Section Redundant Supplied Power Distribution, AUX Continuity, and JBOX IO

***

### 1\. Electrical Connections

(1.1) Leave All Connections as is

(1.2) Hookup These additional Electrical Connections

| Test Enclosure Cable | JBOX Port |
|----------------------|-----------|
| UNSURE | UNSURE |
| UNSURE | UNSURE |

```yaml test-field
name: Electrical Connections Made and Verified
type: boolean

validation: true
continue_on_fail: false
```

***


### 2\. Execute Redundant Supplied Automated Test


(2.1) run function set

``` python borescript FunctionSet tbclib
async def run_test():
    base_test = JboxTestStandBaseTest()
    await base_test.Read_Redundant_Supplied_Test_IO()

# TestFunctionSet tbclib

if __name__ == "__main__":
    asyncio.run(run_test())

```

```yaml test-field
name: Number of Redundant Supplied Tests Passed
type: number

validation:
    low: 19
    high: 19

required: true
```

***

### 3\. Perform Estop Test

(3.1) Actuate and Unactuate the JBOX Door Estop, Ensure that Lights on Channel 1 and 2 of the EP1908 Block inside the JBOX Turn on and off with the estop

```yaml test-field
name: Input Lights on the Beckhoff Block are turning on and off with the estop
type: boolean

validation: true
continue_on_fail: false
```


