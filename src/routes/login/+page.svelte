<script>
    // import { useNavigate } from 'svelte-navigator';
    import { goto } from '$app/navigation';
    import axios from 'axios';
  
    let email = '';
    let password = '';
    let error = '';
    // const navigate = useNavigate();
  
    const login = async () => {
      try {
        const response = await axios.post(`${import.meta.env.VITE_API_URL}/auth/token`, { email, password });
        if (response.status === 200) {
          localStorage.setItem('token', response.data.token);
          // navigate('/dashboard');
          goto('/dashboard')
        } else {
          error = 'Invalid credentials';
        }
      } catch (err) {
        error = 'Invalid email or password';
      }
    };
  </script>
  
  <div class="min-h-screen flex justify-center items-center bg-gray-100">
    <div class="max-w-md w-full bg-white p-6 rounded-lg shadow">
      <h1 class="text-2xl font-semibold text-center mb-4">Login</h1>
      {#if error}<p class="text-red-500 text-sm">{error}</p>{/if}
      <form on:submit|preventDefault={login} class="space-y-4">
        <div>
          <label for="email" class="block text-sm font-medium">Email</label>
          <input type="email" id="email" class="form-input w-full" bind:value={email} required />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium">Password</label>
          <input type="password" id="password" class="form-input w-full" bind:value={password} required />
        </div>
        <button type="submit" class="w-full py-2 bg-blue-600 text-white rounded-lg">Enter</button>
      </form>
    </div>
  </div>
  