## Circulations with Demands and Lower Bounds

These extensions model several sources, several sinks, and edges that *must* carry some flow. Each one reduces back to ordinary [[concepts/network-flow/max-flow-and-ford-fulkerson|max flow]].

### Demands

Each node has a demand dᵥ. The lecture's sign convention is dᵥ = f_in(v) − f_out(v):

- dᵥ < 0: a **source** that supplies −dᵥ;
- dᵥ = 0: an internal node;
- dᵥ > 0: a **sink** that consumes dᵥ.

A feasible flow satisfies 0 ≤ f(e) ≤ cₑ and f_in(v) − f_out(v) = dᵥ for every v.

- **Necessary condition:** if a feasible flow exists, then ∑ dᵥ = 0, so total supply equals total demand, D. This is necessary but *not* sufficient.
- **Reduction:**
  - add a super source s* with an edge s* → v of capacity −dᵥ for each supply node;
  - add a super sink t* with an edge v → t* of capacity dᵥ for each demand node.
- **Test:** a feasible circulation exists exactly when the max flow from s* to t* equals D.

### Lower Bounds

Each edge now has bounds ℓₑ ≤ f(e) ≤ cₑ.

1. Start with f₀(e) = ℓₑ on every edge. This satisfies the capacities but may violate the demands. Let Lᵥ = f₀_in(v) − f₀_out(v).
2. Build G′:
   - capacities c′ₑ = cₑ − ℓₑ, with lower bound 0;
   - demands d′ᵥ = dᵥ − Lᵥ.
   This can create new sources and sinks.
3. Solve the demand-only problem on G′, which reduces to max flow as above, to get f′.
4. The answer is f(e) = f′(e) + ℓₑ.

This is a chain of reductions: lower bounds → demands → max flow.

### Survey Design

There are n customers and k products. Customer i must be asked about between cᵢ and c′ᵢ products they bought. Product j must appear in between pⱼ and p′ⱼ surveys.

**Network:**

- customer i → product j, for each product i bought, with bounds [0, 1];
- s → customer i with bounds [cᵢ, c′ᵢ];
- product j → t with bounds [pⱼ, p′ⱼ];
- t → s with bounds [∑ cᵢ, ∑ c′ᵢ];
- every demand is 0.

A feasible circulation is a valid survey design: flow on (i, j) means customer i is asked about product j.

### Airline Scheduling

k planes must cover m flight segments. One plane can fly segment i and then segment j if there is time to service it, and to reposition it if j departs from a different airport.

**Network:** each unit of flow is one plane.

- each flight is an edge with bounds [1, 1], so it must be flown;
- feasible flight-to-flight connections have bounds [0, 1];
- s → each flight's origin and each destination → t, with bounds [0, 1];
- an edge s → t with bounds [0, k] for planes that stay idle;
- demands d_s = −k and d_t = k.

**Pitfall:** sign conventions for demand vary between sources. Check whether "positive" means supply or consumption before building the super-source edges.

Source: `Lecture 14_ Network Flow and Combinatorial Optimization.pdf`, PDF pp. 125–179. PDF pp. 126–141 restate demands and lower bounds in a different slide style, with a worked example on pp. 133 and 138.
