% Kex at SBFT 2023 Tool Competition
% Azat Abdullin

# Kex\footnote{Azat Abdullin and Vladimir Itsykson. 2022. Kex: A platform for analysis of JVM programs. Information and Control Systems 1 (2022), 30–43. \url{http://www.ius.ru/index.php/ius/article/view/15201}}


* a platform for analysis of JVM programs
	* mainly focused on automatic test generation
* based on symbolic execution
	* also has a concolic execution engine
* research prototype, under development
* third time participation in SBST/SBFT tool competition


################################################################################

# Kex overiview

![](kex)


################################################################################

# Kfg\footnote{\url{https://github.com/vorpal-research/kfg}}: CFG for JVM bytecode

:::::::::::::: {.columns}
::: {.column width="25%"}
\center
![](kfg)
:::
::: {.column width="40%"}
\vspace{5ex}
* class management
* CFG in SSA form
* bytecode analysis and transformation
:::
::::::::::::::


################################################################################

# Predicate state: IR for symbolic transformations

:::::::::::::: {.columns}
::: {.column width="50%"}
\tiny
```java
(
  @S kotlin/jvm/internal/Intrinsics.checkNotNullParameter(arg$0, 'a')
  @S kotlin/jvm/internal/Intrinsics.checkNotNullParameter(arg$1, 'b')
  @S term166 = *(arg$0.x)
  @S term355 = *(arg$1.x)
  @S term587 = term166 < term355
  @S term1050 = term355 > 0
  @S term1368 = new java/lang/IllegalArgumentException
  @S throw term1368
) -> (
  @P arg$0 == null = false
  @P arg$0 instanceof org/example/Point = true
  @P arg$1 == null = false
  @P arg$1 instanceof org/example/Point = true
  @P term587 = true
  @P term1050 = false
)
```
:::
::: {.column width="50%"}
\vspace{8ex}
* symbolic representation of a program
* SMT-specific transformations
:::
::::::::::::::


################################################################################

# Constraint solving

* PS allows support of multiple "backend" solvers
	* Z3, Boolector, CVC4, KSMT
* SBFT configuration used KSMT\footnote{\url{https://github.com/UnitTestBot/ksmt}}
	* efficient asyncronos API for Z3 solver


################################################################################

# JUnit test case generation

\tiny
```java
public class PointKt_foo_408172348_throw_java_lang_IllegalArgumentException2 {
    @Rule public Timeout globalTimeout = new Timeout(100, TimeUnit.SECONDS);
    Object term7711;
    Object term7751;

    @Before
    public void setup() throws Throwable {
        term7711 = newInstance(Class.forName("org.example.Point"));
        setIntField(term7711, term7711.getClass(), "x", -2147483648);
        term7751 = newInstance(Class.forName("org.example.Point"));
        setIntField(term7751, term7751.getClass(), "x", -2147483647);
    }

    @Test
    public void test() throws Throwable {
        Class<?> klass = Class.forName("org.example.PointKt");
        Class<?>[] argTypes = new Class<?>[2];
        argTypes[0] = Class.forName("org.example.Point");
        argTypes[1] = Class.forName("org.example.Point");
        Object[] args = new Object[2];
        args[0] = term7711;
        args[1] = term7751;
        callMethod(klass, "foo", argTypes, null, args);
    }
};
```


################################################################################

# Kex-rt\footnote{\url{https://github.com/AbdullinAM/kex-rt}}: Java standard library approximations

* approximations for standard library of Java 8
* simplifies the semantics of standard library classes
* contains approximations for
	* collections
	* primitive type wrappers
	* string buffers
	* etc.


################################################################################

# Kex-symbolic\footnote{\url{https://github.com/vorpal-research/kex/releases/tag/sbft2023}}

* traditional symbolic execution engine for automatic test generation
* traverses the CFG of PUT on a basic block level
* uses breadth-first search for path selection
	* proof-of-concept prototype

TODO maybe image


################################################################################

# Kex-concolic\footnote{\url{https://github.com/vorpal-research/kex/releases/tag/sbft2023}}


* traditional concolic engine for automatic test generation
* uses Kfg instrumentation to collect symbolic state during concrete execution
* uses Easy-Random\footnote{\url{https://github.com/j-easy/easy-random}} library for initial seed generation
* uses context-guided search for path exploration

TODO maybe image



################################################################################

# Results


\begin{table}[]
\begin{tabular}{|r|cc|cc|}
\hline
                            & \multicolumn{2}{c|}{\textbf{Kex-symbolic}} & \multicolumn{2}{c|}{\textbf{Kex-concolic}} \\ \hline
\textbf{Metric}             & \multicolumn{1}{c|}{\textbf{30 s}} & \textbf{120 s} & \multicolumn{1}{c|}{\textbf{30 s}} & \textbf{120 s} \\ \Xhline{1.5pt}
Line coverage, \%           & \multicolumn{1}{c|}{53.2} & 59.5  & \multicolumn{1}{c|}{57.0} & 65.3  \\ \hline
Branch coverage, \%         & \multicolumn{1}{c|}{38.9} & 47.5  & \multicolumn{1}{c|}{35.0} & 50.0  \\ \hline
Mutation coverage, \%       & \multicolumn{2}{c|}{0.0}          & \multicolumn{2}{c|}{0.0}          \\ \hline
Test case understandability & \multicolumn{2}{c|}{3.95}         & \multicolumn{2}{c|}{3.69}         \\ \Xhline{1.5pt}
Overall ranking             & \multicolumn{2}{c|}{4.89}         & \multicolumn{2}{c|}{3.69}         \\ \hline
\end{tabular}
\end{table}


################################################################################

# Conclusions

TODO: image


################################################################################

# Contact information

<!-- link -->
email:

* <azat.abdullin@jetbrains.com>

repository:

* <https://github.com/vorpal-research/kex>

\vspace{15mm}

<!-- columns -->
:::::::::::::: {.columns}
::: {.column width="30%"}
\vspace{-3ex}
![](jetbrainsResearch)
:::
::::::::::::::

################################################################################

