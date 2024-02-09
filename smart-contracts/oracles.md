# Oracles

Source code: [sources/oracle.move](https://github.com/pontem-network/liquidswap\_v1/blob/main/sources/oracle.move)

In the protocol, Oracles are responsible for storing historical time-weighted average values. Whenever a new pool is created, an oracle is also created for that specific pool. Each oracle comprises samples, with a default of 2 samples per oracle. Each sample contains the following data:

* Timestamp when the sample was created.
* Seconds elapsed since the last update.
* Cumulative active bin ID.
* Cumulative value accumulated.
* Cumulative amount of bins crossed.

The structures created during pool creation are as follows:

```rust
 /// Contains information about oracle sample.
struct Sample has store, copy, drop {
    // Timestamp when sample created.
    filled_at: u64,
    // Seconds elapsed from creation to last update.
    lifetime_in_secs: u64,

    // Cumulative data.
    cum_active_id: u256,
    cum_vol_accum: u256,
    cum_bins_crossed: u256,
}

/// Oracle data itself.
struct Oracle<phantom X, phantom Y, phantom BinStep> has key {
    // Current active sample id.
    active_sample_id: u64,
    // Length of oracle (e.g. amount of samples), counts all samples: even which never filled.
    length: u64,
    // Active length of oracle (e.g. amount of samples filled at least once).
    active_length: u64,
    // Samples itself.
    samples: table::Table<u64, Sample>,

    // Events.
    increase_length_event_handler: event::EventHandle<IncreaseLengthEvent>
}
```

Each new Oracle instance created on resource account and contains a table full of samples.&#x20;

Amounts of samples only limited to `65535` and default 2 samples can be resized to max amount or just by adding another one sample or several of them by anyone. The idea is to save a gas and if someone needs to get access to large historical data the storage can be always resized. If new sample created and doesn't have any data yet, such sample wouldn't be able to query.

The sample filled go one after one in sequence, once the latest sample filled, the 0 sampel will become active.

<figure><img src="../.gitbook/assets/Blank diagram (21).png" alt=""><figcaption></figcaption></figure>

Each sample has a 120 seconds lifetime, during this lifetime every swap would fill the current sample, once lifetime is passed the new sample will be filled. In case of composition fee event during liquidity provision the oracle can be updated too.

You can query each sample by an ID use the module, it would return your data provided in the sample. As each sampel contains previous cumulative data, it's simple allows you to get averaged values, e.g.:

```
timestamp_1 = ...
timestamp_2 = ...
sample_1 = binary_search(timestamp_1)
sample_2 = binary_search(timestamp_2)

time weighted active bin = (sample_1.cum_active_id - sample_2.cum_active_bin_id) / timestamp_1 - timestamp_2
```

The `binary_search` funcs allows you to effective find an sample which nearest to the provided timestamp. It never fails and if sample timestamp is greater than recent saved it would return the latest timestamp, and for past timestamp which already is overwritten it would return the oldest sample.

See more example in [tests](https://github.com/pontem-network/liquidswap\_v1/blob/main/tests/oracle\_tests.move) and try yourself in our integration tutorials.
