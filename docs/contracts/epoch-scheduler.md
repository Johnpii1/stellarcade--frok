# epoch-scheduler

## Public Methods

### `init`
Initialise the epoch scheduler contract.

```rust
pub fn init(env: Env, admin: Address, epoch_duration: u32) -> Result<(), Error>
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |
| `admin` | `Address` |
| `epoch_duration` | `u32` |

#### Return Type

`Result<(), Error>`

### `current_epoch`
View the current epoch based on ledger sequence.

```rust
pub fn current_epoch(env: Env) -> u64
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |

#### Return Type

`u64`

### `schedule_task`
Schedule a task for a future or current epoch.

```rust
pub fn schedule_task(env: Env, task_id: Symbol, epoch: u64, payload_hash: BytesN<32>) -> Result<(), Error>
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |
| `task_id` | `Symbol` |
| `epoch` | `u64` |
| `payload_hash` | `BytesN<32>` |

#### Return Type

`Result<(), Error>`

### `mark_executed`
Mark a task as executed. Restricted to Admin.

```rust
pub fn mark_executed(env: Env, task_id: Symbol) -> Result<(), Error>
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |
| `task_id` | `Symbol` |

#### Return Type

`Result<(), Error>`

### `task_state`
Query the state of a task.

```rust
pub fn task_state(env: Env, task_id: Symbol) -> Option<TaskData>
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |
| `task_id` | `Symbol` |

#### Return Type

`Option<TaskData>`

### `epoch_snapshot`
Returns a timing snapshot combining the current epoch, the next epoch boundary ledger, and the ledger at which the current epoch began (last rollover).

Returns `None` when the contract has not yet been initialised, making the uninitialized state explicit.

```rust
pub fn epoch_snapshot(env: Env) -> Option<EpochSnapshot>
```

#### Parameters

| Name | Type |
|------|------|
| `env` | `Env` |

#### Return Type

`Option<EpochSnapshot>`

#### `EpochSnapshot` fields

| Field | Type | Description |
|-------|------|-------------|
| `current_epoch` | `u64` | Epoch active at the current ledger sequence |
| `epoch_duration` | `u32` | Number of ledgers per epoch as configured at init |
| `current_epoch_start_ledger` | `u64` | Ledger sequence at which the current epoch started (last rollover) |
| `next_epoch_start_ledger` | `u64` | Ledger sequence at which the next epoch will begin |

#### Derivation

Current epoch state is derived entirely from the current ledger sequence:
- `current_epoch = ledger.sequence / epoch_duration`
- `current_epoch_start_ledger = current_epoch × epoch_duration`
- `next_epoch_start_ledger = current_epoch_start_ledger + epoch_duration`

No additional on-chain storage is required.

