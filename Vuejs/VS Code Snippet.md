# Modifying/Adding new snippets in VS Code.
Press ctrl + shift + P  
Type ``` Preferences: Configure User Snippet ```  
Choose language.  

#### Modify file

	"Vue Component Template with Script First": {
		  "prefix": "vscriptfirst",
		  "body": [
			"<script setup>",
			"$1",
			"</script>",
			"$3",
			"<template>",
			"$1",
			"</template>",
			"$3",
			"<style scoped>",
			"$1",
			"</style>"
		  ],
		  "description": "Vue component structure with script first, followed by template and style."
		}
	  }
