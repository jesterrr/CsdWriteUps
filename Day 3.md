
# CTF

Santa’s ElfTV license key checker got leaked! Finally, a break for a broke elf like you, starving for that sweet, sweet elf dopamine. The catch? You’ve got to reverse-engineer Santa’s “state-of-the-art” security to unlock it. Think you’re smarter than the guy who still uses reindeer for transportation? Prove it and claim your ElfTV fix!!!!

Connect using `nc ctf.csd.lol 1001`
# Solution

The Source code for the ctf is:

```rust
use std::fs::File;
use std::io::{self, BufRead, Write};
use std::path::Path;

fn supasecurefibberdachicheckerthing(n: usize) -> Vec<u64> {
    let mut fib: Vec<u64> = vec![0, 1];
    for i in 2..n {
        let next = fib[i - 1].checked_add(fib[i - 2]).unwrap_or(0);
        fib.push(next);
    }
    fib
}

fn validate_license_key(key: &str) -> bool {
    if !key.starts_with("XMAS") {
        return false;
    }

    if key.len() != 12 {
        return false;
    }

    let ascii_sum: u32 = key.chars().skip(4).take(5).map(|c| c as u32).sum();
    if ascii_sum != 610 {
        return false;
    }

    let fib_482 = supasecurefibberdachicheckerthing(483)[482];
    let fib_last_3 = fib_482 % 1000;

    let key_last_3: u16 = match key[9..12].parse() {
        Ok(num) => num,
        Err(_) => return false,
    };

    if key_last_3 != fib_last_3 as u16 {
        return false;
    }

    true
}

fn win() {
    let flag_path = Path::new("flag.txt");

    if let Ok(file) = File::open(flag_path) {
        let mut buf_reader = io::BufReader::new(file);
        let mut flag = String::new();
        if buf_reader.read_line(&mut flag).is_ok() {
            println!("🎄 Ho Ho Ho!, go watch some ELFTV!: {}", flag.trim());
        } else {
            println!("smth went wrong contact vip3r with error (flag-file-1)");
        }
    } else {
        println!("smth went wrong contact vip3r with error (flag-file-2)");
    }
}


fn main() {
    println!("🎄 Welcome to the ElfTV XMAS-license key checker!");
    println!("Please enter your license key:");

    let stdin = io::stdin();
    let mut input = String::new();
    let mut stdout = io::stdout();

    if stdin.read_line(&mut input).is_ok() {
        let key = input.trim();
        if validate_license_key(key) {
            win();
        } else {
            writeln!(stdout, "Ho ho ho! Try again.").unwrap();
        }
    } else {
        writeln!(stdout, "Failed to read the input!").unwrap();
    }
}
```

According to the code the key should have this structure:
-  Staring with "XMAS"
-  5 ASCII characters that add up to 610 with their values
-  Ending with the last three digits of the 482nd Fibonacci number.

So I created a Keygen:

```rust
use std::fs::File;
use std::io::{self, BufRead, Write};
use std::path::Path;

fn supasecurefibberdachicheckerthing(n: usize) -> Vec<u64> {
    let mut fib: Vec<u64> = vec![0, 1];
    for i in 2..n {
        let next = fib[i - 1].checked_add(fib[i - 2]).unwrap_or(0);
        fib.push(next);
    }
    fib
}

fn main() {
    let fib_482 = supasecurefibberdachicheckerthing(483)[482];
    let fib_last_3 = fib_482 % 1000;
    let prefix = "XMAS";
    let suffix = format!("{:03}", fib_last_3);
    let target_sum = 610;
    let mut middle = String::new();
    let mut current_sum = 0;
    for i in 0..5 {
        for char_code in 32..127 {
            let potential_sum = current_sum + char_code;
            if potential_sum <= target_sum {
                middle.push(char::from_u32(char_code).unwrap());
                current_sum = potential_sum;
                break;
            }
        }
        if middle.len() < i+1{
            panic!("err1");
        }
    }
    let diff = target_sum as i32 - current_sum as i32;
    if diff != 0 {
        for i in 0..5 {
            let mut new_char_code = middle.as_bytes()[i] as i32 + diff/5;
            if new_char_code < 32{
                new_char_code = 32;
            } else if new_char_code > 126{
                new_char_code = 126;
            }
            middle.replace_range(i..i+1, &String::from(char::from_u32(new_char_code as u32).unwrap()));
        }
    }
    let actual_sum: u32 = middle.chars().map(|c| c as u32).sum();
    if actual_sum != 610{
        panic!("err2");
    }

    let key = format!("{}{}{}", prefix, middle, suffix);
    println!("Generated Key: {}", key);
}
```

The output : `Generated Key: XMASzzzzz738`

after running the nc and inputing the flag:

```ssh
jstr$ nc ctf.csd.lol 1001
🎄 Welcome to the ElfTV XMAS-license key checker!
Please enter your license key:
XMASzzzzz738
🎄 Ho Ho Ho!, go watch some ELFTV!: csd{Ru57y_L1c3N53_k3Y_CH3Ck3r}
```


The answer is: `csd{Ru57y_L1c3N53_k3Y_CH3Ck3r}`
