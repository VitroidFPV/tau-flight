<script>
    import {
      Tab,
      TabGroup,
      TabList,
      TabPanel,
      TabPanels,
    } from "@rgossiaux/svelte-headlessui";
    // @ts-ignore
    import Basic from "/src/components/pages/Basic.svelte";
  </script>
  
  <div class="px-32 py-16 w-full h-screen flex flex-col items-center justify-center">
    <h1 class="text-yellow-400 font-bold text-6xl mb-3 self-start">TauFlight</h1>
    <TabGroup class="w-full h-full flex-col" defaultIndex={0}>
        <TabList class="relative border-zinc-100">
            <Tab class={({selected}) => `tab ${selected ? "tab-active" : "tab-inactive"}`}>Basic</Tab>
            <Tab class={({selected}) => `tab ${selected ? "tab-active" : "tab-inactive"}`}>Radio</Tab>
            <Tab class={({selected}) => `tab ${selected ? "tab-active" : "tab-inactive"}`}>VTX</Tab>
            <Tab class={({selected}) => `tab ${selected ? "tab-active" : "tab-inactive"}`}>Rate/PID</Tab>
        </TabList>
        <TabPanels class="bg-zinc-100 rounded-2xl min-h-96 p-8 shadow-2xl">
            <TabPanel><Basic /></TabPanel>
            <TabPanel>Content 2</TabPanel>
            <TabPanel>Content 3</TabPanel>
        </TabPanels>
    </TabGroup>
</div>
