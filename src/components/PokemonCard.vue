<template>
  <div
    class="border-2 border-gray-300 rounded-lg flex flex-col items-center h-[100%] capitalize font-bold text-lg shadow-lg shadow-gray-500 font-serif cursor-pointer"
  >
    <div class="border-b-2 border-gray-300 w-[100%] text-center py-4">
      <slot name="title"></slot>
    </div>
    <div class="w-[15rem] p-4">
      <slot name="content"></slot>
    </div>
    <div class="border-t-2 border-gray-300 w-[100%] text-center py-4">
      <slot name="footer"></slot>
    </div>
  </div>
</template>
