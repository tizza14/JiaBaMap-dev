<template>
<div class="sm:w-full sm:mx-16 my-4 flex full-width screen-padding items-center">
  <div class="flex flex-nowrap relative">
    <div class="absolute w-12 h-12 top-0 left-4 shadow-md rounded-md sm:relative sm:w-28 sm:h-28">
    <img class="w-full h-full rounded-md object-cover" src="https://ap-south-1.linodeobjects.com/nidin-production-v3/store/icons/s_15961_icon_20230725_145324_87a73.jpg" alt="店家品牌">
  </div>

  <div class="mt-16 mx-4 flex item-center flex-col space-y-2 ">
    <div class="flex items-center space-x-2">
      <h2 class="text-base sm:text-xl">得正 台北東湖計劃</h2>
      <font-awesome-icon :icon="['fas', 'heart']" 
      class="text-sm text-gray-400"
      />
    </div>
    
    <div class="flex items-center space-x-2">
      <font-awesome-icon :icon="['fas', 'star']" 
      class="text-sm text-yellow-400"
      />
      <p class="text-sm text-gray-500">3.6</p>
      <p class="text-sm text-gray-500">(6)</p>
      <font-awesome-icon :icon="['fas', 'clock']" 
      class="text-sm text-gray-500"
      />
      <p class="text-sm text-gray-500">11:00 ~ 20:00</p>
    </div>

    <div class="flex items-center space-x-2">
      <font-awesome-icon :icon="['fas', 'location-dot']" 
      class="text-sm text-gray-500"
      />
      <p class="text-sm text-gray-500">3.1公里</p>
      <a href="#" class="text-sm text-amber-500">台北市內湖區東湖路133號</a>
      <font-awesome-icon :icon="['fas', 'phone']" 
      class="text-sm text-gray-500"
      />
      <p class="text-sm text-amber-500">02-2345-6789</p>
    </div>

  </div>
  </div>
  <div class="items-center overflow-x-auto">
    <ul class="flex flex-row items-center justify-center">
      <li>曾經點過</li>
      <li>雙柚新品</li>
      <li>Original Tea</li>
      <li>Double Fruit</li>
      <li>Cheese Milk Foam</li>
      <li>Classic Milk Tea</li>
      <li>Fresh Milk</li>
      <li>購物袋</li>
    </ul>
  </div>
 
</div>

</template>