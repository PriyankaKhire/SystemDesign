<h1>Concurrency</h1>
  <p>
    It is the execution of the multiple instruction sequences at the same time.
  </p>
  <h2>Pros</h2>
    <ul>
      <li>Can perform multiple operations at the same time.</li>
      <li>Saves time since applications are run in parallel</li>
    </ul>
  <h2>Cons</h2>
    <ul>
      <li> If two processes both make use of a global variable and both perform read and write on that variable, then the order iin which various read and write are executed is critical.</li>
    </ul>
<h1>Atomic and Non-atomic properties</h1>
  <h2>Atomic</h2>
    <ul>
      <li>Only one thread access the variable (static type)</li>
      <li>Therefore it is thread safe.</li>
      <li>It is slow in performance</li>
    </ul>
  <h2>Non-atomic</h2>
    <ul>
      <li>Multiple thread access the variable (dynamic type).</li>
      <li>Not thread safe</li>
      <li>Fast in performance</li>
    </ul>
<h1>Multiversion concurrency control</h1>
    
