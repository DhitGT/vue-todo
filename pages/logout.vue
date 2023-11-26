<template></template>
<script setup>
const router = useRouter();
const supabase = useSupabaseClient();
const signOut = async () => {
  const { error } = await supabase.auth.signOut();
  router.push("/");
  if (error) console.log(error);
};
signOut();
</script>
