<script>
    // @ts-ignore
    import Header1 from "/src/components/main/Header1.svelte";
    // @ts-ignore
    import Paragraph from "/src/components/main/Paragraph.svelte";
    // @ts-ignore
    import SmallText from "/src/components/main/SmallText.svelte";
    // @ts-ignore
    import Number from "/src/components/main/Number.svelte";
    // @ts-ignore
    import Help from "/src/components/main/Help.svelte";
    import { QuestionMarkCircleIcon } from "@rgossiaux/svelte-heroicons/solid";
    import { Switch } from "@rgossiaux/svelte-headlessui";
    import {
    Listbox,
    ListboxButton,
    ListboxOptions,
    ListboxOption,
    Transition,
  } from "@rgossiaux/svelte-headlessui";

  const ESCProtocols = [
    { id: 1, name: "DShot150", unavailable: false },
    { id: 2, name: "DShot300", unavailable: false },
    { id: 3, name: "DShot600", unavailable: false },
    { id: 4, name: "MultiShot", unavailable: true },
    { id: 5, name: "PWM", unavailable: false },
  ];

  const PIDFreqs = [
    { id: 1, name: "8kHz", unavailable: false },
    { id: 2, name: "4kHz", unavailable: false },
    { id: 3, name: "2kHz", unavailable: false },
    { id: 4, name: "2.67kHz", unavailable: false },
    { id: 5, name: "1.66kHz", unavailable: false },
    { id: 6, name: "1.33kHz", unavailable: false },
    { id: 7, name: "1kHz", unavailable: false },
  ]

    let selectedESCProtocol = ESCProtocols[0];
    let selectedPIDFreq = PIDFreqs[0];

    let PropsOut = false;
    let Leds = false;
    let AirMode = true;
    let MotorStop = false;
    let BiDir = false;
</script>

<div class="flex flex-col">
    <Header1 text="Basic Settings">
        <QuestionMarkCircleIcon class="h-6 w-6 mt-2 ml-4 hover:fill-yellow-400 duration-300"/>
    </Header1>
    <Paragraph text="The basic betaflight settings like..." />
    <div class="options flex justify-between mt-4">
        <div class="flex flex-col w-1/2 h-full pr-16">
            <div id="VariousSwitches" class="flex justify-between">
                <div class="flex mr-2">
                    <div>
                        <Switch
                            checked={Leds}
                            on:change={(e) => (Leds = e.detail)}
                            class={Leds ? "switch switch-enabled" : "switch switch-disabled"}>
                            <span class="sr-only">Enable LED Support</span>
                            <span class="toggle" class:toggle-on={Leds} class:toggle-off={!Leds} />
                        </Switch>
                        <SmallText text="LED Strip" />
                    </div>
                    <Help text="Enable support for LED strips, set up with a basic pattern. <br>To change it, choose one in the 
                    other tab, or in Betaflight configurator" />
                </div>
                <div class="flex mr-2">
                    <div>
                        <Switch
                            checked={AirMode}
                            on:change={(e) => (AirMode = e.detail)}
                            class={AirMode ? "switch switch-enabled" : "switch switch-disabled"}>
                            <span class="sr-only">Enable Air Mode</span>
                            <span class="toggle" class:toggle-on={AirMode} class:toggle-off={!AirMode} />
                        </Switch>
                        <SmallText text="Air Mode" />
                    </div>
                    <Help text="Gives better authority at 0 throttle, generally useful to keep control in the air throughought puchouts" />
                </div>
                <div class="flex mr-4">
                    <div>
                        <Listbox class="relative w-32 z-50 mr-2" value={selectedPIDFreq} on:change={(e) => (selectedPIDFreq = e.detail)}>
                            <ListboxButton class="border-b-2 border-zinc-600 focus:border-yellow-400 focus:outline-none hover:border-yellow-400 w-full duration-300" >{selectedPIDFreq.name}</ListboxButton>
                            <Transition
                            enter="transition duration-300 ease-out origin-top"
                            enterFrom="transform scale-y-0"
                            enterTo="transform scale-y-100"
                            leave="transition duration-300 ease-out origin-top"
                            leaveFrom="transform scale-y-100"
                            leaveTo="transform scale-y-0">
                        
                            <ListboxOptions class="absolute backdrop-blur-[6px] shadow-2xl w-full rounded-b-2xl border-b-4 border-yellow-400">
                                {#each PIDFreqs as PIDFreq (PIDFreq.id)}
                                  <ListboxOption class={({ active }) => (active ? "dropdown-active" : "dropdown")} value={PIDFreq} disabled={PIDFreq.unavailable}>
                                    {PIDFreq.name}
                                  </ListboxOption>
                                {/each}
                              </ListboxOptions>
                            </Transition>
                          </Listbox>
                          <SmallText text="PID Loop Freq"/>
                    </div>
                    <Help text="Frequency of the PID loop calculations. Usually F411 FCs can do 4KHz, F405 do 4/8KHz, and F722 and above can do 8KHz easily" />
                </div>
            </div>
            <div id="CraftName" class="flex h-16 items-start mt-4">
                <input type="text" class="input bg-transparent border-b-2 border-zinc-600 focus:border-yellow-400 focus:outline-none focus:border-b-4 hover:border-b-4 duration-300 w-40" id="CraftName"/>
                <label for="CraftName" class="ml-2 w-32">Craft Name</label>
                <div class="h-40 w-40 bg-contain duration-100" class:props-in={!PropsOut} class:props-out={PropsOut}></div>
            </div>
        </div>

        <div class="flex flex-col w-1/2 h-full">
            <Paragraph text="Motors/ESC" />
            <div class="flex items-end justify-start mb-4">

                <div class="flex mr-4">
                        <Listbox class="relative w-64 z-50 mr-2" value={selectedESCProtocol} on:change={(e) => (selectedESCProtocol = e.detail)}>
                            <ListboxButton class="border-b-2 border-zinc-600 focus:border-yellow-400 focus:outline-none hover:border-yellow-400 w-full duration-300" >{selectedESCProtocol.name}</ListboxButton>
                            <Transition
                            enter="transition duration-300 ease-out origin-top"
                            enterFrom="transform scale-y-0"
                            enterTo="transform scale-y-100"
                            leave="transition duration-300 ease-out"
                            leaveFrom="transform scale-y-100"
                            leaveTo="transform scale-y-0">
                        
                            <ListboxOptions class="absolute backdrop-blur-[6px] shadow-2xl w-full rounded-b-2xl border-b-4 border-yellow-400">
                                {#each ESCProtocols as ESCProtocol (ESCProtocol.id)}
                                  <ListboxOption class={({ active }) => (active ? "dropdown-active" : "dropdown")} value={ESCProtocol} disabled={ESCProtocol.unavailable}>
                                    {ESCProtocol.name}
                                  </ListboxOption>
                                {/each}
                              </ListboxOptions>
                            </Transition>
                          </Listbox>
                          <SmallText text="ESC Protocol"/>
                </div>

                  <div class="flex">
                      <div class="mr-2">
                          <Number id="MotorPoles" value={14} min={4} max={32} step={1} text="Motor Poles" />
                      </div>
                      <Help text="Used for the bidirectional DShot rpm measurement, most often 14. You can find this in the manufacturer specsheet" />
                  </div>
                  
            </div>
            <div id="ESCSwitches" class="flex justify-between items-center">
                <div class="flex">
                    <div>
                        <Switch
                            checked={PropsOut}
                            on:change={(e) => (PropsOut = e.detail)}
                            class={PropsOut ? "switch switch-enabled" : "switch switch-disabled"}>
                            <span class="sr-only">Enable Props Out</span>
                            <span class="toggle" class:toggle-on={PropsOut} class:toggle-off={!PropsOut} />
                        </Switch>
                        <SmallText text="Props out" />
                    </div>
                    <Help text="Default is motors and props going in towards the camera, props out go away from the camera" />
                </div>
                <div class="flex">
                    <div>
                        <Switch
                            checked={MotorStop}
                            on:change={(e) => (MotorStop = e.detail)}
                            class={MotorStop ? "switch switch-enabled" : "switch switch-disabled"}>
                            <span class="sr-only">Motor Stop</span>
                            <span class="toggle" class:toggle-on={MotorStop} class:toggle-off={!MotorStop} />
                        </Switch>
                        <SmallText text="Motor Stop" />
                    </div>
                    <Help text="Motors won't spin at zero throttle" />
                </div>
                <div class="flex">
                    <div>
                        <Switch
                            checked={BiDir}
                            on:change={(e) => (BiDir = e.detail)}
                            class={BiDir ? "switch switch-enabled" : "switch switch-disabled"}>
                            <span class="sr-only">BiDirectional DShot</span>
                            <span class="toggle" class:toggle-on={BiDir} class:toggle-off={!BiDir} />
                        </Switch>
                        <SmallText text="BiDir DShot" />
                    </div>
                    <Help text="FC can get RPM information from the ESCs, makes for better filtering options.<br>You need your ESC to run BiDir firmware" />
                </div>
                <div class="flex">
                    <div class="mr-2">
                        <Number id="MotorIdle" value={5.5} min={0} max={100} step={0.1} text="Motor Idle (%)" />
                    </div>
                    <Help text="Throttle % that the motors will spin when idle" />
                </div>
            </div>
            <div id="GyroAlignment" class="flex flex-col justify-between mt-4">
                <div class="flex">
                    <div class="mr-2">
                        <Paragraph text="Gyro Alignment" />
                    </div>
                    <Help text="If your board is rotationally offset, you can correct each axis" />
                </div>
                <div class="flex justify-between">
                    <Number id="RolDeg" min="-180" max="180" step="5" text="Roll"></Number>
                    <Number id="PitchDeg" min="-180" max="180" step="5" text="Pitch"></Number>
                    <Number id="YawDeg" min="-180" max="180" step="5" text="Yaw"></Number>
                    <div class="flex">
                        <div class="mr-2">
                            <Number id="MaxArmAngle" min="0" max="180" step="5" text="Max Arm Angle"></Number>
                        </div>
                        <Help text="Quad won't arm if tilted above the selected value (180 disables the check)" />
                    </div>
                </div>
            </div>
            <div class="mt-4">Basic settings for Betaflight. Hover over the question marks next to the settings to find definitions</div>
        </div>
    </div>
</div>