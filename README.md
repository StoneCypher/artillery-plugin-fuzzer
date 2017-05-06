
# Artillery Fuzzer - easy fuzzing for HTTP APIs

This plugin makes it dead-easy to run some fuzz testing on your HTTP API with [Artillery](https://artillery.io).

Use it to send unexpected and weird payloads to your endpoints, see what breaks and fix it to make your application more **secure** and more **resilient**.

The payloads generated by this plugin are based on the awesome [Big List Of Naughty Strings](https://github.com/minimaxir/big-list-of-naughty-strings/).

## Usage

**Important**: this plugin requires Artillery `v1.5.8-4` or later.

Install the plugin with:

```
npm install artillery-plugin-fuzzer
```

Enable the plugin in your test script with:

```yaml
config:
  plugins:
    fuzzer: {}
```

Then just use the `{{ naughtyString }}` variable as you would any other variable in your scenario:

```yaml
- post:
    url: "/session"
    json:
      username: "{{ naughtyString }}"
      password: "secret"
```

A new value for the `naughtyString` variable will be generated for each new request in a scenario.

See the complete example in [`example.yml`](example.yml)

# Why?

Runnning a quick test with this plugin against your app's backend can help uncover bugs, security issues and QA problems.

## A Real World Example

Here's a sample payload sent by this plugin:

👾 🙇 💁 🙅 🙆 🙋 🙎 🙍

Something innocent like this could crash your application if it persists data in a MySOL database using the default settings. How? MySQL InnoDB engine uses the `latin1` encoding by default.

Did you set the `utf8` encoding on your database? You're still in trouble because those characters are outside the [BMP](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) and you need to have specified `utf8mb4` and potentially made changes to your schema to be able to store them properly.

Modern software systems are incredibly complex. [If you haven't tried it, assume it's broken](https://landing.google.com/sre/book/chapters/testing-reliability.html).

Happy fuzzing!

# Roadmap

Sending [bnls](https://github.com/minimaxir/big-list-of-naughty-strings/) payloads is a good start for a fuzzer, but it's only the first small step. We want to make Artillery a great tool for API fuzz testing. Got an idea for this plugin? Share your feedback in [Issues](issues).

# License

MPL 2.0