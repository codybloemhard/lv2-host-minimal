# lv2-host-minimal
Simple library to host lv2 plugins.
Is not meant to support any kind of GUI.

- [x] Host fx plugins (audio in, audio out)
- [x] Set parameters
- [x] Host midi instruments (midi in, audio out)

## Note
I build this library when no other LV2 hosting crate for Rust existed.
I am not really all that knowledgeable on LV2 and I want you to know that there is a slightly differnt
but overall better Rust LV2 hosting crate available since: [https://github.com/wmedrano/livi-rs](https://github.com/wmedrano/livi-rs).
Please check it out, to see if it might suit your goals better.
If you still want to contribute here, or report bugs, feel free to do so of course.

## Example

```rust
use lv2hm::Lv2Host;

let mut host = Lv2Host::new(1, 1, 44100);
host.add_plugin("http://calf.sourceforge.net/plugins/Monosynth", "Organ".to_owned()).expect("Lv2hm: could not add plugin");
host.set_value("Organ", "MIDI Channel", 0.0);

for i in 0..44100 {
    // alternate midi on and off messages, 5000 samples apart
    let mut midimsg = Vec::new();
    if (i % 10000) == 0 {
        midimsg.push((0, [0x90, 72, 96]))
    }
    else if (i % 5000) == 0 {
        midimsg.push((0, [0x80, 72, 96]))
    }
    let out = host.apply_multi(0, midimsg, [&[0.0], &[0.0]]).unwrap();
    // do something with your audio
    // here, `out` will contain one sample for each stereo channel.
}
```

## License
```
Copyright (C) 2022 Cody Bloemhard

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```
