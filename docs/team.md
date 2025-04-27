---
layout: page
title: Rencontrer l'équipe
description: Le développement de Vite est mené par une équipe internationale.
---

<script setup>
import {
  VPTeamPage,
  VPTeamPageTitle,
  VPTeamPageSection,
  VPTeamMembers
} from 'vitepress/theme'
import { core, emeriti } from './_data/team'
</script>

<VPTeamPage>
  <VPTeamPageTitle>
    <template #title>Meet the Team</template>
    <template #lead>
        Le développement de Vite est mené par une équipe internationale, certains d'entre eux ont choisi d'être présentés ci-dessous.
    </template>
  </VPTeamPageTitle>
  <VPTeamMembers :members="core" />
  <VPTeamPageSection>
    <template #title>Team Emeriti</template>
    <template #lead>
      Ici, nous honorons certains membres (qui ne sont plus actif) de l'équipe ayant fait des contributions précieuses par le passé.
    </template>
    <template #members>
      <VPTeamMembers size="small" :members="emeriti" />
    </template>
  </VPTeamPageSection>
</VPTeamPage>
