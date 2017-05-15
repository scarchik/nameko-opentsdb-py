# nameko-opentsdb-py
OpenTSDB (through opentsdb-py) dependency for nameko services

## Installation

Install
```bash
pip3 install nameko-opentsdb-py
```

Install latest version from Git:
```bash
pip3 install git+https://github.com/fraglab/nameko-opentsdb-py.git
```

## Usage

## Use with dependency providers

SharedOpenTSDB - use OpenTSDB with dependency providers.

```python
from nameko.extensions import DependencyProvider
from nameko_opentsdb import SharedOpenTSDB


class ServiceLogicProvider(DependencyProvider):
    tsdb = SharedOpenTSDB()

```

## Use with services

### OpenTSDBBasic
Base class to use OpenTSDB with service, provide access to [opentsdb-py](https://github.com/scarchik/opentsdb-py) functionality.

### OpenTSDB
Extend basic functionality with predefined nameko rpc call metrics:

* Counter('nameko.rpc_call.success', ['method'])
* Counter('nameko.rpc_call.failed', ['method'])
* Gauge('nameko.rpc_call.execute_time_milliseconds', ['method'])

```python
from nameko_opentsdb import OpenTSDB


class Service:
    tsdb = OpenTSDB()

```


## Example:
```python
from nameko.rpc import rpc
from nameko_opentsdb import OpenTSDB
from opentsdb import Counter


class ProfilesService:
    name = 'profiles_service'

    tsdb = OpenTSDB(
        CREATED=Counter('profiles.created'),
    )

    @rpc
    def create(self):
        # Some logic
        self.tsdb.CREATED.inc()
        return True

```