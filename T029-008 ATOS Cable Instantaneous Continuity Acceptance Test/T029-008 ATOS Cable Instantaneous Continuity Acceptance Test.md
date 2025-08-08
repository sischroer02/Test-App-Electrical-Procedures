# T029-005 ATOS Cable Instantaneous Continuity Acceptance Test

``` yaml boring-test
machine: T029 - Electrical
testname: T029-005 ATOS Cable Instantaneous Continuity Acceptance Test
classification: T029 - Electrical
```

## Purpose

Electrically Test the Custom ATOS Cable for Continuity and Water Ingress Protection over Time


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


ATOS_CABLE_TEST_LIBRARY = {
    1: {'ATOS PIN' : 1, 'OUTPUT_VARIABLE_NAME' : 'J302_1', 'INPUT_VARIABLE_NAME' : 'J1101_1', 'XV' : True},
    2: {'ATOS PIN' : 2, 'OUTPUT_VARIABLE_NAME' : 'J302_2', 'INPUT_VARIABLE_NAME' : 'J1101_2', 'XV' : True},
    3: {'ATOS PIN' : 3, 'OUTPUT_VARIABLE_NAME' : 'J302_3', 'INPUT_VARIABLE_NAME' : 'J1101_3', 'XV' : True},
    4: {'ATOS PIN' : 4, 'OUTPUT_VARIABLE_NAME' : 'J302_4', 'INPUT_VARIABLE_NAME' : 'J1101_4', 'XV' : True},
    5: {'ATOS PIN' : 5, 'OUTPUT_VARIABLE_NAME' : 'UNDEFINED', 'INPUT_VARIABLE_NAME' : 'J1101_5', 'XV' : True}
}




class AtosTestBase:
    def __init__(self):
        self.testvariable = True
        self.service = BroccoliSymbolService(broc_url=BROC_URL, logger=logger)
        self.NUM_TESTS_PASSED_ATOS = 0
        self.NUM_TESTS_ATOS = 5
        self.fail = False


    async def Read_ATOS_Test_IO(self):
        try:
            # Initialize the service
            self.NUM_TESTS_PASSED_ATOS = 0
            await self.service.initialize()
            logger.info("Broccoli service initialized")

            for key, entry in ATOS_CABLE_TEST_LIBRARY.items():
                self.input_variable_name = entry['INPUT_VARIABLE_NAME']
                self.output_variable_name = entry['OUTPUT_VARIABLE_NAME']
                self.input_symbol = SymbolHandle(self.input_variable_name)
                self.output_symbol = SymbolHandle(self.output_variable_name)
                self.expected_value = entry['XV']

                self.service.write_symbol(self.output_symbol, True)
                await asyncio.sleep(2)
                self.current_value = self.service.read_symbol(self.input_symbol)
                self.service.write_symbol(self.output_symbol, False)

                logger.info("Read symbol %s: %s", self.current_symbol.name, bool(self.current_value))

                if self.expected_value == self.current_value:
                    # RESULT.set_metadata(ATOS_CABLE_TEST_LIBRARY[key]["TEST_NAME"], ATOS_CABLE_TEST_LIBRARY[key]["Pass Value"])
                    # RESULT.save_delta()
                    # logger.info("Pass result saved")
                    self.NUM_TESTS_PASSED_ATOS += 1
                else:
                    # RESULT.set_metadata(ATOS_CABLE_TEST_LIBRARY[key]["TEST_NAME"], ATOS_CABLE_TEST_LIBRARY[key]["Fail Value"])
                    # RESULT.save_delta()
                    # logger.info("Fail result saved")

        except Exception as e:
            logger.error("Error reading symbols: %s", e)
            RESULT.set_metadata("Read Error", str(e))
            RESULT.save_delta()

        finally:
            logger.info(self.NUM_TESTS_PASSED_ATOS)
            if (self.NUM_TESTS_PASSED_ATOS < self.NUM_TESTS_ATOS) or self.fail:
                self.fail = True
                RESULT.set_metadata('ATOS Test Result', False)
                RESULT.save_delta()
            else:
                RESULT.set_metadata('ATOS Test Result', True)
                RESULT.save_delta()
            # Clean up
            try:
                await self.service.close()
                logger.info("Broccoli service closed")
            except Exception as e:
                logger.error("Error closing service: %s", e)
        

```

***

## Test Setup

***

### 1\. Test Rules

(1.1) 1 operator is needed for this test

(1.3) Disconnect Power Supply Before making any connections, do not make live connections.

```yaml test-field
name: Rules Understood
type: boolean

validation: true
continue_on_fail: false
```

***

## Primary Test Section

***

### 1\. Initial Conditions

(1.1) Hook up ATOS Cable to tester

(1.2) Fully Submerge Cable Connection Point to the bottom of the barrel

```yaml test-field
name: Initial Conditions Confirmed
type: boolean

validation: true
continue_on_fail: false
```

***

### 2\. Continuity for Duration Test

(2.1) Run Duration Test

``` python borescript FunctionSet tbclib
async def run_test():
    base_test = AtosTestBase()
    await base_test.Read_ATOS_Test_IO()


if __name__ == "__main__":
    asyncio.run(run_test())

```

``` yaml test-field
name: ATOS Recurring Test Result
type: boolean

validation: true
required: true
```