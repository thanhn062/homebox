<script setup lang="ts">
  import { useI18n } from "vue-i18n";
  import MdiHome from "~icons/mdi/home";
  import MdiViewList from "~icons/mdi/view-list";
  import MdiPackageVariant from "~icons/mdi/package-variant";
  import MdiPlus from "~icons/mdi/plus";
  import MdiDotsHorizontal from "~icons/mdi/dots-horizontal";
  import {
    DropdownMenu,
    DropdownMenuContent,
    DropdownMenuItem,
    DropdownMenuTrigger,
  } from "@/components/ui/dropdown-menu";
  import { DialogID } from "~/components/ui/dialog-provider/utils";
  import { useDialog } from "~/components/ui/dialog-provider";
  import { useSidebar } from "@/components/ui/sidebar/utils";

  const { t } = useI18n();
  const route = useRoute();
  const { openDialog } = useDialog();
  const { toggleSidebar } = useSidebar();

  const tabs = computed(
    () =>
      [
        { label: t("menu.home"), to: "/home", icon: MdiHome },
        { label: t("global.items"), to: "/search", icon: MdiViewList },
        { label: t("menu.locations"), to: "/locations", icon: MdiPackageVariant },
      ] as const
  );

  const isActive = (path: string) => route.path === path || (path !== "/home" && route.path.startsWith(`${path}/`));

  const openCreateItem = () => openDialog(DialogID.CreateEntity, { params: { baseType: "item" } });
  const openCreateLocation = () => openDialog(DialogID.CreateEntity, { params: { baseType: "location" } });
  const openCreateTag = () => openDialog(DialogID.CreateTag);
</script>

<template>
  <nav
    aria-label="Mobile navigation"
    class="fixed inset-x-5 bottom-[calc(env(safe-area-inset-bottom)+0.25rem)] z-30 mx-auto max-w-lg rounded-full border bg-background/95 shadow-xl backdrop-blur lg:hidden"
  >
    <div class="grid h-[4.25rem] grid-cols-5 items-center px-1.5">
      <NuxtLink
        :to="tabs[0].to"
        class="flex min-w-0 flex-col items-center justify-center gap-1 rounded-full py-1.5 text-muted-foreground transition-colors hover:text-foreground focus-visible:z-10 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
        :class="{ 'text-primary': isActive(tabs[0].to) }"
        :aria-current="isActive(tabs[0].to) ? 'page' : undefined"
      >
        <component :is="tabs[0].icon" class="size-5 shrink-0" aria-hidden="true" />
        <span class="max-w-full truncate text-[0.68rem] leading-none">{{ tabs[0].label }}</span>
      </NuxtLink>

      <NuxtLink
        :to="tabs[1].to"
        class="flex min-w-0 flex-col items-center justify-center gap-1 rounded-full py-1.5 text-muted-foreground transition-colors hover:text-foreground focus-visible:z-10 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
        :class="{ 'text-primary': isActive(tabs[1].to) }"
        :aria-current="isActive(tabs[1].to) ? 'page' : undefined"
      >
        <component :is="tabs[1].icon" class="size-5 shrink-0" aria-hidden="true" />
        <span class="max-w-full truncate text-[0.68rem] leading-none">{{ tabs[1].label }}</span>
      </NuxtLink>

      <DropdownMenu>
        <DropdownMenuTrigger as-child>
          <button
            type="button"
            class="mx-auto flex size-12 flex-col items-center justify-center rounded-full bg-primary text-primary-foreground shadow-md transition-transform hover:scale-105 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2"
            :aria-label="t('global.create')"
          >
            <MdiPlus class="size-6 shrink-0" aria-hidden="true" />
          </button>
        </DropdownMenuTrigger>
        <DropdownMenuContent side="top" align="center" class="z-[60] mb-2">
          <DropdownMenuItem @click="openCreateItem">{{ t("menu.create_item") }}</DropdownMenuItem>
          <DropdownMenuItem @click="openCreateLocation">{{ t("menu.create_location") }}</DropdownMenuItem>
          <DropdownMenuItem @click="openCreateTag">{{ t("menu.create_tag") }}</DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>

      <NuxtLink
        :to="tabs[2].to"
        class="flex min-w-0 flex-col items-center justify-center gap-1 rounded-full py-1.5 text-muted-foreground transition-colors hover:text-foreground focus-visible:z-10 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
        :class="{ 'text-primary': isActive(tabs[2].to) }"
        :aria-current="isActive(tabs[2].to) ? 'page' : undefined"
      >
        <component :is="tabs[2].icon" class="size-5 shrink-0" aria-hidden="true" />
        <span class="max-w-full truncate text-[0.68rem] leading-none">{{ tabs[2].label }}</span>
      </NuxtLink>

      <button
        type="button"
        class="flex min-w-0 flex-col items-center justify-center gap-1 rounded-full py-1.5 text-muted-foreground transition-colors hover:text-foreground focus-visible:z-10 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring"
        :aria-label="t('menu.more')"
        @click="toggleSidebar"
      >
        <MdiDotsHorizontal class="size-5 shrink-0" aria-hidden="true" />
        <span class="max-w-full truncate text-[0.68rem] leading-none">{{ t("menu.more") }}</span>
      </button>
    </div>
  </nav>
</template>
