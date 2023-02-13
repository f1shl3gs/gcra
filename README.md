# gcra -- Generic Cell Rate Algorithm

This repo is just a simple `GCRA` implement, it's NOT a rate-limiter library.
You can always implement one of your own, to meet your all needs.


For example
```rust
/// RateLimiter is not thread safe.
pub struct RateLimiter {
    quota: Quota,
    state: GcraState,
}

impl RateLimiter {
    pub fn new(quota: Quota) -> Self {
        Self {
            quota,
            state: Default::default(),
        }
    }

pub fn check(&mut self) -> bool {
    self.state.check_and_modify(&self.quota, 1).is_ok()
}
}

#[test]
fn chek() {
    let quota = Quota::new(4, Duration::from_secs(4));
    let mut limiter = RateLimiter::new(quota);

    for _i in 0..4 {
        assert!(limiter.check());
    }
    assert!(!limiter.check());

    std::thread::sleep(Duration::from_secs(1));
    assert!(limiter.check());
}
```
