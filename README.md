# Notes on IC3

##### Border cubes manipulation by propagate

```
../IC3.cpp
295:      CubeSet borderCubes;  // additional cubes in this and previous frames
304:    // does not set 'borderCubes' of 'frame'.
662:      pair<CubeSet::iterator, bool> rv = frames[level].borderCubes.insert(cube);
734:      earliest = k+1;  // earliest frame with enlarged borderCubes
776:        set_difference(fr.borderCubes.begin(), fr.borderCubes.end(),
780:          cout << i << " " << fr.borderCubes.size() << " " << rem.size() << " ";
781:        fr.borderCubes.swap(rem);
786:        for (CubeSet::const_iterator i = fr.borderCubes.begin();
787:             i != fr.borderCubes.end(); ++i)
797:        for (CubeSet::iterator j = fr.borderCubes.begin();
798:             j != fr.borderCubes.end();) {
809:            fr.borderCubes.erase(tmp);
818:        if (fr.borderCubes.empty())
```

#### Error occurence:

```cpp
Model.cpp
81:// constraint /\ primed constraint /\ transition /\ ~error
100:    sslv->setFrozen(varOfLit(error()).var(), true);
101:    sslv->setFrozen(varOfLit(primedError()).var(), true);
113:    require.insert(_error);
116:    prequire.insert(_error);
149:    // assert ~error, constraints, and primed constraints
150:    // should this not be ~error' ?
151:    sslv->addClause(~_error);
216:void Model::loadError(Minisat::Solver & slv) const {
218:  require.insert(_error);
318:  // acquire error from given propertyIndex

IC3.cpp
277:      size_t depth;  // Length of CTI suffix to error.
404:        cls.push(~model.primedError());
727:    // Strengthens frontier to remove error successors.
734:        // solve for error successor, given transition relation.
736:        bool rv = frontier.consecution->solve(model.primedError());
742:        // handle CTI with error successor
860:    model.loadError(*base0);
861:    bool rv = base0->solve(model.error());
868:    rv = base1->solve(model.primedError());

Model.h
86:// primed error constraint.  Variables are kept aligned with the
92:  // functions, the error, and the AND table, closely reflecting the
103:    _error(_err), inits(NULL), sslv(NULL)
111:    // same with primed error
112:    _primedError = primeLit(_error);
186:  // Error and its primed form.
187:  Minisat::Lit error() const { return _error; }
188:  Minisat::Lit primedError() const { return _primedError; }
197:  // Loads the TR into the solver.  Also loads the primed error
198:  // definition such that Model::primedError() need only be asserted
200:  // negation of the error are always added --- except that the primed
207:  // Loads the error into the solver, which is only necessary for the
209:  void loadError(Minisat::Solver & slv) const;
225:  const Minisat::Lit _error;
226:  Minisat::Lit _primedError;
```

