<script>

    import {page} from '$app/stores'
    import {goto} from '$app/navigation'
</script>

<header class="bg-white border-b">
  <div class="flex items-center justify-between h-16 px-4">
    <div class="flex items-center space-x-4">
      <div class="text-lg font-semibold text-indigo-700">SmartQP</div>
      <!-- <h1 class="text-xl font-medium">Dashboard</h1> -->
    </div>
    
    <div class="flex items-center space-x-4">
      <div class="relative">
        <input 
          type="search"
          placeholder="Search"
          class="w-64 pl-10 pr-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />
        <span class="absolute left-3 top-2.5 text-gray-400">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
          </svg>
        </span>
      </div>
      
      <button class="p-2 rounded-full hover:bg-gray-100">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
          <path d="M10 2a6 6 0 00-6 6v3.586l-.707.707A1 1 0 004 14h12a1 1 0 00.707-1.707L16 11.586V8a6 6 0 00-6-6zM10 18a3 3 0 01-3-3h6a3 3 0 01-3 3z" />
        </svg>
      </button>
      
      <button class="p-1 rounded-full hover:bg-gray-100">
        <img
          src="https://ui-avatars.com/api/?name=User&background=random"
          alt="User"
          class="h-8 w-8 rounded-full"
        />
      </button>
    </div>
  </div>
</header>