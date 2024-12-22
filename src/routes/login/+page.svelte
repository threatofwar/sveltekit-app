<script>
   let username = '';
   let password = '';
   let message = '';
   let isLoading = false;

   const handleLogin = async () => {
      console.log('Username:', username);
      console.log('Password:', password);

      isLoading = true;
      try {
         const username = 'testuser';
         const password = 'testpassword';
         const response = await fetch('http://192.168.100.135:8080/login', {
            method: 'POST',
            headers: {
               'Content-Type': 'application/json',
            },
            body: JSON.stringify({ username, password }),
         });

         const data = await response.json();

         if (response.ok) {
            // Save tokens or handle them
            localStorage.setItem('access_token', data.access_token);
            localStorage.setItem('refresh_token', data.refresh_token);
            message = 'Login successful!';
         } else {
            message = data.message || 'Login failed!';
         }
      } catch (error) {
         message = 'An error occurred during login.';
      } finally {
         isLoading = false;
      }
   };
</script>

<main>
   <h1>Login</h1>

   <form on:submit|preventDefault={handleLogin}>
      <div>
         <label for="username">Username</label>
         <input
            type="text"
            id="username"
            bind:value={username}
            placeholder="Enter your username"
            required
         />
      </div>

      <div>
         <label for="password">Password</label>
         <input
            type="password"
            id="password"
            bind:value={password}
            placeholder="Enter your password"
            required
         />
      </div>

      {#if isLoading}
         <p>Loading...</p>
      {/if}

      <button type="submit" disabled={isLoading}>Login</button>
   </form>

   {#if message}
      <p>{message}</p>
   {/if}
</main>

<style>
   main {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      text-align: center;
   }

   input {
      margin: 10px 0;
      padding: 10px;
      width: 200px;
   }

   button {
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
   }

   button:disabled {
      background-color: #6c757d;
   }
</style>
