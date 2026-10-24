# okay-effects

**Algebraic effects with control and audit.** A program's type says what it may do, a handler decides how it is done,
and every effect passes a point where it can be recorded, changed or refused. A sensitive effect, such as a payment,
cannot run outside its audit, and the compiler checks that.

The same design exists in Scala, Java, Go, Rust, Python and TypeScript. A protocol between them lets a program written
in one language run under handlers written in another, so a system spread over several platforms can be watched and
audited at one point.

**Site: [okay-effects.github.io](https://okay-effects.github.io)**

- [Start with okay-simple](https://okay-effects.github.io/start.html): algebraic effects in Scala in six short steps
- [For business](https://okay-effects.github.io/business.html): audit by construction, what DORA, MiFID II, PSD2, GDPR and the AI Act ask of software
- [Scala and Spark](https://okay-effects.github.io/scala-spark.html): Scala 3 on Spark, ZIO and Cats in one program, audit added to an existing Spark job
- [From a monolith to services](https://okay-effects.github.io/modularity.html): one handler at a time

## Try it

```scala
resolvers += "okay-effects" at "https://okay-effects.github.io/okay-simple/maven"
libraryDependencies += "io.github.okay-effects" %% "okay-effects-machine" % "0.1.0"
```

```scala
import okay.effects.*
import okay.effects.std.*

object counter extends std.On[Reader % Int + State % Int]:
  def tick: Int ! Row =
    for
      step <- ask[Int]
      n    <- get
      _    <- put(n + step)
    yield n + step

run(state(0)(reader(5)(counter.tick)))   // (5,5)
```

## Repositories

| | |
|---|---|
| **Start here** | |
| [okay-simple](https://github.com/okay-effects/okay-simple) | Algebraic effects for Scala 3 (3.3 LTS on) and Scala 2.13, one interface. **Released, 0.1.1.** Streams, async, parsing, logic, transactions; interop with ZIO, cats-effect, fs2, kyo, monix |
| [okay-scala](https://github.com/okay-effects/okay-scala) | Algebraic effects for Scala 3 alone: the same rows, one CEK machine, enums and GADTs, no cast; JVM, Scala.js, Native. **Released, 0.1.0.** okay-simple's interface runs on it |
| [okay-kotlin](https://github.com/okay-effects/okay-kotlin) | Algebraic effects for Kotlin: effects as interfaces and context parameters on the coroutines' own continuations, or as data on okay-scala's CEK machine — one program, two engines. Multi-shot, interception; JVM, JS, native. **Released, 0.1.0** |
| **Applications and their screens** | |
| [okay-ui](https://github.com/okay-effects/okay-ui) | Business screens as data, Scala 3: forms, tables, buttons; the screen is an effect, the toolkit its handler. A reference app, [Expenses — live in the browser](https://okay-effects.github.io/okay-ui/expenses/) |
| [Coinpath](https://okay-effects.github.io/coinpath/) | **Live:** follow a payment in a blockchain, in the browser. Each step with its reason, exchanges named from their own proof-of-reserves lists. Built on okay-scala and okay-ui |
| [okay-faces](https://github.com/okay-effects/okay-faces) | One program, every face: terminal, Swing, HTML, REST, MCP tools for an agent, Laminar in the browser. **Released, 0.1.0** |
| **The kernel and the protocol** | |
| [okay-core](https://github.com/okay-effects/okay-core) | The kernel, Scala 3: one monad, delimited control, multi-shot continuations as data; interceptors and audit |
| [okay-kernel](https://github.com/okay-effects/okay-kernel) | The protocol between implementations, with a TCK every implementation passes |
| **The platform** | |
| [okay](https://github.com/okay-effects/okay) | About a hundred modules on the kernel: HTTP, SQL, streams, cluster, durable workflows, Spark, LLM and MCP, and a bytecode check of the effect boundary |
| **Other languages** | |
| [okay-java](https://github.com/okay-effects/okay-java) | Java 25, on `shift0` |
| [okay-go](https://github.com/okay-effects/okay-go) | Go, direct style on coroutines |
| [okay-rust](https://github.com/okay-effects/okay-rust) | Rust: the kernel ported, and effects on async/await |
| [okay-python](https://github.com/okay-effects/okay-python) | Python, typed, on generators |
| [okay-ts](https://github.com/okay-effects/okay-ts) | TypeScript, effects tracked in the type |
| **Tools** | |
| [symdex](https://github.com/okay-effects/symdex) | Structural code intelligence for coding agents, served over MCP |

Apache License 2.0. Author: [Sergiy Shcherbyna](https://www.linkedin.com/in/sergiy-shcherbyna-b904751), open to work as a team
member or contract consultant in Scala and JVM projects.
