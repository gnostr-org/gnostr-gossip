# Gossip

Gossip is a desktop client for nostr.

Nostr is "Notes and Other Stuff Transmitted by Relays."

NOTE: After two false starts (tauri, gtk4) I'm moving to egui, which should be much easier
and faster to develop.

## Status

This is pre-alpha code. It is not ready for use.

If you want to use it anyway, you will need to do a few things manually to get it started (do this after the Building and Installing section below):

- Sqlite3 to your gossip.sqlite file (see the About page to find where it is)
- Insert people in the person table with followed=1
- Insert at least one person_relay entry for each person
- There may be other steps needed. Development is happening fast. Feel free to ask a question
  by opening a github issue. I'm not snotty about it, you can just chat with me on github
  issues if you want.

After that, it should start following those people. You may need a restart from time to
time as it loses connections to relays still, and some live event handling is less thorough
than startup event handling is.

## Features

- Asychronous design: No busy waiting or polling.
- Portable. The UI will run on anything that runs one of these backends: OpenGL (glium, glow), OpenGL ES (glow, wgpu), WebGL (glow), Vulkan (wgpu), Metal (wgpu), DirectX 11/12 (wgpu), Browsers (WebAssembly). And rust runs very many places.
- Talks to as few relays as it needs to to keep up with the people you follow, and doesn't overload them with too heavy of requests.

## nostr features supported

This section will need updating.
- Reads and displays type=1 (TextNote) events in a feed sorted by time created.
    - Shows replies under the message they reply to
    - Shows deleted events struck out with red deleted marker.
- Shows people you subscribe to in the Person tab
    - Processes type=0 (Metadata) events to show user information on events in your feed (name, avatar)
    - Lets you subscribe to a person (via public key and relay), but currently requires a restart
- Identity:
    - Lets you generate an ID (private key) and stores that key encrypted under a passphrase, zeroing memory when finished with it where it can.
    - Lets you import an ID (private key)
- Settings: feed chunk size, overlap, autofollow, but these don't work right yet.

## nostr features planned for first alpha release

- [ ] Create your identity
- [ ] Follow people and see their feed posts in time order
- [ ] See your feed in threaded mode
- [ ] Choose and manage relays to post to
- [ ] Post messages and reactions
- [ ] Show reactions

## nostr features planned for subsequent releases

- [ ] Lets you subscribe to a person via a DNS ID (NIP-35)
- [ ] Validates users via NIP-05
- [ ] Lets you react to other people's posts
- [ ] Lets you show events from people you don't follow if they reply to a post you do
- [ ] Lets you mute someone
- [ ] Lets you rank relays
- [ ] Shows links as links
- [ ] Shows images inline (option to wait for your approval)
- [ ] Include a 'client' tag
- [ ] Show the 'client' tag of posts
- [ ] Support "content-warning"
- [ ] Allow browsing of relay-global events of people you dont follow
- [ ] Multiple identities
- [ ] Publish your following list
- [ ] Follow someone privately (without including in your posted following list)
- [ ] Allow viewing of other people's following lists w/ petnames
- [ ] Dismiss a message for this session only w/o deleting it

## Building and Installing

### Step 1 - Install Rust

If you don't already have rust installed, follow the guidance at [rust-lang.org](https://www.rust-lang.org/). Most people install rust under their user account, rather than using system packages which often don't keep up to date. Also, cargo caches compilation artifacts in your home directory.

### Step 2 - Clone this Repository

````bash
$ git clone https://github.com/mikedilger/gossip
````

### Step 3 - Compile

````bash
$ cd gossip
$ cargo build --release
````

The output will be a binary executable in `target/release/gossip`

Everything gossip needs (fonts, icons) is baked into this executable. It doesn't need to find assets. So you can move it and run it from anywhere.

To make the binary smaller

````base
$ strip gossip
````

This binary should be portable to similar systems with similar hardware and operating system.

If you want a binary optimized for your exact processor with the newest features enabled:

````bash
$ RUSTFLAGS="-C target-cpu=native --cfg tokio_unstable" cargo build --release
````


## Technology Involved

- Rust Language
- egui Rust GUI framework
- SQLite 3
- Tungstenite websocket library
- Tokio async task runtime
- Serde serialization/deserialization
- Many others

## License

 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, shall be licensed as above, without any additional
terms or conditions.
