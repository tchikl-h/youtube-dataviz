<!-- App.svelte -->
<script>
	import { Router, Route } from "svelte-routing";
  
	// Admin Layout
	import Admin from "./layouts/Admin.svelte";
	// Auth Layout
	import Auth from "./layouts/Auth.svelte";
  
	export let url = "";
  </script>
  
  <Router url="{url}">
	<!-- admin layout -->
	<Route path="/" component="{Admin}" />
	<!-- auth layout -->
	<Route path="auth/*auth" component="{Auth}" />
  </Router>