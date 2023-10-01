<script>
  import TailwindCss from './lib/TailwindCSS.svelte';
  import { Router, Link, Route } from "svelte-routing";
  
  import Home from './routes/Home.svelte';
  import Notecard from './routes/Notecard.svelte';
  import Search from './routes/Search.svelte';
  import Signup from './routes/Signup.svelte';
  import Login from './routes/Login.svelte';
  import Create from './routes/Create.svelte';

  export let url = "";
</script>

<TailwindCss />

<div class="bg-gradient-to-br from-blue-950 to-blue-600 min-h-screen min-w-screen h-max x-max font-mono text-white">
  <Router {url}>
    <nav class="sticky top-0 right-0 inline-flex items-center h-16 w-full z-10 max-w-full">
      <div class="inline-flex relative items-center gap-x-10 ml-10 text-2xl">
        <Link to="/" class="hover:scale-110">Home</Link>
        <Link to="/search" class="hover:scale-110">Search</Link>
        <Link to="/signup" class="hover:scale-110">Signup</Link>
        <Link to="/login" class="hover:scale-110">Login</Link>
        <Link to="/create" class="hover:scale-110">Create Notecard</Link>
      </div>
    </nav>

    <div>
      <Route path="/create">
        <Create />
      </Route>
      <Route path="/login">
        <Login />
      </Route>
      <Route path="/signup">
        <Signup />
      </Route>
      <Route path="/search">
        <Search />
      </Route>
      <Route path="/notecards/:id" let:params>
        <Notecard notecardID={params.id} />
      </Route>
      <Route path="/">
        <Home />
      </Route>
    </div>
  </Router>
</div>