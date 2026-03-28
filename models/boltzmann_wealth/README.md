# Boltzmann Wealth Model

## What the Model Does and Why I Chose It

This model is a classic agent-based model called the **Boltzmann Wealth Model**. It simulates how wealth gets distributed among a group of people.

The rule is simple: every agent starts with 1 unit of money. Each step, every agent randomly picks someone else and gives them 1 unit (as long as they still have money). No one has any advantage, the rules are exactly the same for everyone.

But after running it, the wealth distribution becomes very uneven. Most agents end up poor, and a few get rich. This distribution is called the **Boltzmann-Gibbs distribution** in physics, and it looks a lot like real-world wealth inequality.

I chose this model because the result is really interesting to me: everyone starts equal, everyone follows the same rules, but inequality still shows up on its own. This is not something you'd expect just by thinking about it, but the simulation data shows it to you directly.

## Mesa Features Used

- `mesa.Agent` / `CellAgent`: defines what each agent does (hold wealth, move, exchange)
- `mesa.Model`: manages the whole simulation world
- `AgentSet.shuffle_do()`: randomly shuffles the order agents act in each step, so no agent always goes first
- `Agent.create_agents()`: creates many agents at once
- `Model.run_for()`: runs the model for a set number of steps
- `OrthogonalMooreGrid`: a 10×10 grid where agents can only trade with others in the same cell
- `mesa.DataCollector`: records the Gini coefficient and each agent's wealth every step, making it easy to plot later

## What I Learned Building It

Before starting, I thought building an agent-based model would be pretty complex. But once I actually got into it, I realized the whole thing is built up one small step at a time.

The first bit of code I wrote was super simple, just creating one agent and having it call `say_hi()` to print a greeting. Then I added `say_wealth()` to check that agents could correctly track their own wealth. After that I wrote the actual `exchange()` logic so agents could transfer money to each other. Then I added a grid so agents had to be in the same spot to trade. Finally I added the DataCollector to record data and used seaborn to make charts.

None of these steps were hard on their own. But after stacking them together, I ended up with a simulation of 100 agents interacting on a grid for hundreds of steps, and the Gini coefficient curve I plotted actually reflects patterns you see in the real world. That process of going from nothing to something meaningful really stuck with me.

A few other things I picked up along the way:

- **shuffle_do vs do**: with `do`, agents always run in the same order, so agent 0 always goes first. `shuffle_do` randomizes the order, which is more fair.
- **Random seeds**: setting `rng=42` makes the results exactly the same every run. Really useful for debugging, and also a basic practice for making research reproducible.
- **Number of agents matters**: with 10 agents the histogram looks messy, but with 100 agents over 100 runs, the exponential decay shape becomes very clear. Sample size really does matter.

## What Was Hard / What Surprised Me / What I'd Do Differently

**What was hard:** Going from the basic version (all agents randomly pair up and trade) to the grid version (agents have to `move()` first and can only trade with "neighbors") meant I had to rethink the interaction logic. It's not just adding a grid, the conditions in `give_money()` had to change too.

**What surprised me:** I knew the result would be unequal, but I didn't expect it to be this extreme. After running the simulation, most agents had a wealth of 0 or 1, and only a very few were rich. It made me start thinking again about what "fair rules" really means.

**What I'd do differently:** If I had more time, I'd like to try changing the parameters and see what happens — for example, instead of giving 1 unit each time, adjusting the amount based on how wealthy the agent is, or making the grid bigger so agents interact less often. I'd also like to build a grid visualization to see whether rich agents cluster together in space — basically, whether "rich neighborhoods" form on their own.
