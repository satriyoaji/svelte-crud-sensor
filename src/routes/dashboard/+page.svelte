<script>
    import { onMount } from 'svelte';
    import axios from 'axios';
  
    /**
	 * @type {any[]}
	 */
    let sensors = [];
    let name = '';
    let type = '';
    let value = '';
    /**
	 * @type {{ id: any; } | null}
	 */
    let editingSensor = null;
  
    const fetchSensors = async () => {
      const response = await axios.get(`${import.meta.env.VITE_API_URL}/sensors`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
      });
      sensors = response.data;
    };
  
    const saveSensor = async () => {
      const payload = { name, type, value };
      if (editingSensor) {
        await axios.put(`${import.meta.env.VITE_API_URL}/sensors/${editingSensor.id}`, payload, {
          headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
        });
      } else {
        await axios.post(`${import.meta.env.VITE_API_URL}/sensors`, payload, {
          headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
        });
      }
      name = type = value = '';
      editingSensor = null;
      fetchSensors();
    };
  
    const deleteSensor = async (/** @type {number} */ id) => {
      await axios.delete(`${import.meta.env.VITE_API_URL}/sensors/${id}`, {
        headers: { Authorization: `Bearer ${localStorage.getItem('token')}` },
      });
      fetchSensors();
    };
  
    const startEditing = (/** @type {{ id: number; name: string; type: string; value: string; }} */ sensor) => {
      editingSensor = sensor;
      name = sensor.name;
      type = sensor.type;
      value = sensor.value;
    };
  
    onMount(fetchSensors);
  </script>
  
  <div class="min-h-screen bg-gray-100 p-8">
    <h1 class="text-2xl font-bold mb-6">Environment Sensors</h1>
    <form on:submit|preventDefault={saveSensor} class="mb-6 bg-white p-4 rounded-lg shadow space-y-4">
      <div>
        <label class="block text-sm font-medium">Name</label>
        <input type="text" bind:value={name} class="form-input w-full" required />
      </div>
      <div>
        <label class="block text-sm font-medium">Type</label>
        <input type="text" bind:value={type} class="form-input w-full" required />
      </div>
      <div>
        <label class="block text-sm font-medium">Value</label>
        <input type="number" bind:value={value} class="form-input w-full" required />
      </div>
      <button type="submit" class="px-4 py-2 bg-green-600 text-white rounded-lg">
        {editingSensor ? 'Update Sensor' : 'Add Sensor'}
      </button>
    </form>
    <table class="w-full bg-white rounded-lg shadow">
      <thead class="bg-gray-200">
        <tr>
          <th class="p-2 text-left">Name</th>
          <th class="p-2 text-left">Type</th>
          <th class="p-2 text-left">Value</th>
          <th class="p-2 text-right">Actions</th>
        </tr>
      </thead>
      <tbody>
        {#each sensors as sensor}
          <tr class="border-t">
            <td class="p-2">{sensor.name}</td>
            <td class="p-2">{sensor.type}</td>
            <td class="p-2">{sensor.value}</td>
            <td class="p-2 text-right">
              <button class="px-2 py-1 bg-blue-600 text-white rounded" on:click={() => startEditing(sensor)}>Edit</button>
              <button class="px-2 py-1 bg-red-600 text-white rounded" on:click={() => deleteSensor(sensor.id)}>Delete</button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  