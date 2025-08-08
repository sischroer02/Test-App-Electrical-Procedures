# T029-006 HPU Displacement CTRL Harness Acceptance Test

``` yaml boring-test
machine: T029 - Electrical
testname: T029-006 HPU Displacement CTRL Harness Acceptance Test
classification: T029 - Electrical
```

## Purpose

Electrically Test the Custom HPU Displacement CTRL Valve Cable for Continuity


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


HPU_CTRL_CABLE_TEST_LIBRARY = {
    1: {'CABLE PIN' : 1, 'OUTPUT_VARIABLE_NAME' : 'S201_1', 'INPUT_VARIABLE_NAME' : 'J1001_1', 'XV' : True},
    2: {'CABLE PIN' : 2, 'OUTPUT_VARIABLE_NAME' : 'S201_3', 'INPUT_VARIABLE_NAME' : 'J1001_2', 'XV' : True}
}




class HPUCableTestBase:
    def __init__(self):
        self.testvariable = True
        self.service = BroccoliSymbolService(broc_url=BROC_URL, logger=logger)
        self.NUM_TESTS_PASSED_HPU = 0
        self.NUM_TESTS_HPU = 2
        self.fail = False


    async def ReadHPUTestIO(self):
        try:
            # Initialize the service
            self.NUM_TESTS_PASSED_HPU = 0
            await self.service.initialize()
            logger.info("Broccoli service initialized")

            for key, entry in HPU_CTRL_CABLE_TEST_LIBRARY.items():
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
                    self.NUM_TESTS_PASSED_HPU += 1
                else:
                    # RESULT.set_metadata(ATOS_CABLE_TEST_LIBRARY[key]["TEST_NAME"], ATOS_CABLE_TEST_LIBRARY[key]["Fail Value"])
                    # RESULT.save_delta()
                    # logger.info("Fail result saved")

        except Exception as e:
            logger.error("Error reading symbols: %s", e)
            RESULT.set_metadata("Read Error", str(e))
            RESULT.save_delta()

        finally:
            logger.info(self.NUM_TESTS_PASSED_HPU)
            if (self.NUM_TESTS_PASSED_HPU < self.NUM_TESTS_HPU) or self.fail:
                self.fail = True
                RESULT.set_metadata('HPU Displacement CTRL VALVE Cable Recurring Test Result', False)
                RESULT.save_delta()
            else:
                RESULT.set_metadata('HPU Displacement CTRL VALVE Cable Recurring Test Result', True)
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

(1.1) Hook up HPU Displacement CTRL Valve Cable to tester

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
    base_test = HPUCableTestBase()
    await base_test.ReadHPUTestIO()


if __name__ == "__main__":
    asyncio.run(run_test())

```

``` yaml test-field
name: HPU Displacement CTRL VALVE Cable Recurring Test Result
type: boolean

validation: true
required: true
```