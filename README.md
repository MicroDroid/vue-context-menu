vue-context-menu
==============

> The plugin is VERY minimal and does NOT provide any styling or anything. This simplifies context menus implementation but that's it.

## Usage

There are **three** steps for using this plugin.

#### Step 1: Create context menu

```vue
<template>
  ...
  <ContextMenu ref="contextMenu">
    interesting stuff
  </ContextMenu>
</template>

<script>
  import ContextMenu from '@overcoder/vue-context-menu';
  
  export default {
    ...
    
    components: {
      ...
      ContextMenu,
    },
  };
</script>
```

#### Step 2: Trigger the menu

```vue
  <ul>
    <li v-for="cats in cat" :key="cat.id"
      @contextmenu.prevent="event => contextMenu.open(event)">
      {{ cat.name }}
    </li>
  </ul>
```

> Test it out, context menu should work now (assuming `cats`).

#### Step 3: Make it useful

Ok first of all you noticed the `event` is passed to `contextMenu.open` and this is to determine mouse location etc.

Now this plugin supports thing called **contexts**, here's an example:

```vue
  <ul>
    <li v-for="cats in cat" :key="cat.id"
      @contextmenu.prevent="event => contextMenu.open(event, cat)">
      {{ cat.name }}
    </li>
  </ul>
```

You can see we provided the `cat` as the second parameter to `contextMenu.open`. We gave it a context. Now you can access this context within the menu itself:

```vue
  <ContextMenu ref="contextMenu">
    <template v-slot="slotProps"> <!-- slotsProps are props passed from child to slot in parent -->
      <!-- Now, here slotProps.ctx does contain the context (cat) BUT -->
      <template v-if="slotProps.ctx"> <!-- we need to add this since context is null on initial render -->
        <!-- Now we can peacefully access slotProps.ctx without getting any errors -->

        {{ slotProps.ctx.name }}
      <template>
    </template>
  </ContextMenu>
```

That's all I think.