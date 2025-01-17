<script setup lang="ts">
import { useForm } from 'vee-validate'
import { toTypedSchema } from '@vee-validate/zod'
import * as z from 'zod'
import { cn } from '@/lib/utils'
import { GuestLayout } from '@/components/layouts'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card'
import { FormControl, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form'
import { Input } from '@/components/ui/input'
import { Loader2 } from 'lucide-vue-next'
import { RouterLink } from 'vue-router'
import { toast } from '@/components/ui/toast'

const formSchema = toTypedSchema(
  z.object({
    email: z
      .string({ required_error: 'Le champ est obligatoire' })
      .email({ message: 'Le champ doit être une adresse e-mail valide' })
  })
)
const { handleSubmit, isSubmitting } = useForm({
  validationSchema: formSchema
})
const onSubmit = handleSubmit(async (values) => {
  try {
    toast({
      title: 'Succès',
      description: `Un mail a été envoyé à l'adresse e-mail ${values.email}.`,
      duration: 5000
    })
  } catch (error) {
    toast({
      title: 'Erreur',
      description: `Nous ne sommes pas parvenus à envoyer un mail à l'adresse e-mail ${values.email}.`,
      duration: 5000
    })
  }
})
</script>

<template>
  <GuestLayout>
    <Card :class="cn('w-[420px]', $attrs.class ?? '')">
      <CardHeader>
        <CardTitle>Pragmap</CardTitle>
        <CardDescription>
          Veuillez renseigner votre adresse e-mail afin de recevoir un lien de réinitialisation de
          mot de passe
        </CardDescription>
      </CardHeader>

      <CardContent>
        <form class="space-y-6" @submit="onSubmit">
          <FormField v-slot="{ componentField }" name="email">
            <FormItem>
              <FormLabel>Adresse e-mail</FormLabel>
              <FormControl>
                <Input type="email" v-bind="componentField" />
              </FormControl>
              <FormMessage />
            </FormItem>
          </FormField>

          <div class="flex flex-col sm:flex-row justify-between">
            <Button v-if="!isSubmitting" type="submit">
              <FontAwesomeIcon class="mr-2" icon="fa-solid fa-paper-plane" />
              Envoyer
            </Button>
            <Button v-else type="disabled">
              <Loader2 class="w-4 h-4 mr-2 animate-spin" />
              Envoi...
            </Button>

            <Button type="button" variant="link" size="sm" as-child>
              <RouterLink to="/login">Retour à la connexion &#8594;</RouterLink>
            </Button>
          </div>
        </form>
      </CardContent>
    </Card>
  </GuestLayout>
</template>
