<script lang="ts">
	// Page simple pour afficher une "méthode personnalisée"
	// avec des tableaux statiques basés sur tes définitions.

	type Row = Record<string, string>;

	const periodiciteRows: Row[] = [
		{ periodicite: 'QuickWin', duree: '0 – 3 mois' },
		{ periodicite: 'Court terme', duree: '3 – 12 mois' },
		{ periodicite: 'Moyen terme', duree: '12 – 18 mois' },
		{ periodicite: 'Long terme', duree: 'Supérieur à 18 mois' },
		{ periodicite: 'Périodique', duree: 'Périodiquement' }
	];

	const complexiteRows: Row[] = [
		{
			complexite: 'Important',
			definition:
				"Complexité à coût important, correspondant à une tâche supérieure à une dizaine de jours/homme et/ou un coût supérieur à une centaine de milliers de dirhams (par exemple, refonte d'une architecture réseau)."
		},
		{
			complexite: 'Moyen',
			definition:
				"Complexité à coût moyen, correspondant à une tâche inférieure à une dizaine de jours/homme et/ou un coût inférieur à une centaine de milliers de dirhams (par exemple, rédaction d'une procédure plus complexe, achat d'un composant, etc.)."
		},
		{
			complexite: 'Faible',
			definition:
				"Complexité à coût faible, n'entraînant pas de coût d'acquisition (par exemple, modification d'un paramètre, rédaction d'une procédure simple, etc.)."
		}
	];

	const typeActionRows: Row[] = [
		{
			type_action: 'Action Organisationnelle',
			description: 'Action visant à modifier les processus ou politiques internes de la SOCIÉTÉ.'
		},
		{
			type_action: 'Action Technique',
			description: 'Intervention sur les SI ou infrastructures techniques de la SOCIÉTÉ.'
		},
		{
			type_action: 'Action Organisationnelle et Technique',
			description: 'Action combinant les deux dimensions (organisationnelle et technique).'
		}
	];

	const prioriteRows: Row[] = [
		{
			echelle: 'Priorité 4',
			definition: 'Risque Faible',
			signification: '[1 – 20['
		},
		{
			echelle: 'Priorité 3',
			definition: 'Risque Modéré',
			signification: '[20 – 36['
		},
		{
			echelle: 'Priorité 2',
			definition: 'Risque Élevé',
			signification: '[36 – 64['
		},
		{
			echelle: 'Priorité 1',
			definition: 'Risque Extrême',
			signification: '[64 – 120]'
		}
	];
</script>

<main class="p-6 space-y-8">
	<section class="space-y-2">
		<h1 class="text-2xl font-bold text-gray-900">Méthode personnalisée</h1>
		<p class="text-gray-600 max-w-3xl">
			Cette page regroupe tes tables de référence (périodicité, complexité, type d’action, priorité)
			en français. Elle n’impacte pas le moteur de scoring standard de CISO Assistant, mais te sert
			de guide opérationnel pour documenter ta propre méthode.
		</p>
	</section>

	<section class="space-y-4">
		<h2 class="text-xl font-semibold text-gray-900">Table 1&nbsp;: Périodicité / Durée</h2>
		<div class="overflow-hidden rounded-lg border border-gray-200 bg-white shadow-sm">
			<table class="min-w-full text-sm">
				<thead class="bg-gray-50">
					<tr>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Périodicité</th>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Durée</th>
					</tr>
				</thead>
				<tbody>
					{#each periodiciteRows as row, i}
						<tr class={i % 2 === 0 ? 'bg-white' : 'bg-gray-50'}>
							<td class="px-4 py-2 text-gray-800">{row.periodicite}</td>
							<td class="px-4 py-2 text-gray-800">{row.duree}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</section>

	<section class="space-y-4">
		<h2 class="text-xl font-semibold text-gray-900">Table 2&nbsp;: Niveau de complexité</h2>
		<div class="overflow-hidden rounded-lg border border-gray-200 bg-white shadow-sm">
			<table class="min-w-full text-sm align-top">
				<thead class="bg-gray-50">
					<tr>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Complexité</th>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">
							Définition du niveau de complexité supposé
						</th>
					</tr>
				</thead>
				<tbody>
					{#each complexiteRows as row, i}
						<tr class={i % 2 === 0 ? 'bg-white' : 'bg-gray-50'}>
							<td class="px-4 py-2 font-medium text-gray-800">{row.complexite}</td>
							<td class="px-4 py-2 text-gray-800">{row.definition}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</section>

	<section class="space-y-4">
		<h2 class="text-xl font-semibold text-gray-900">Table 3&nbsp;: Type de l’action</h2>
		<div class="overflow-hidden rounded-lg border border-gray-200 bg-white shadow-sm">
			<table class="min-w-full text-sm align-top">
				<thead class="bg-gray-50">
					<tr>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Type de l’action</th>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Description</th>
					</tr>
				</thead>
				<tbody>
					{#each typeActionRows as row, i}
						<tr class={i % 2 === 0 ? 'bg-white' : 'bg-gray-50'}>
							<td class="px-4 py-2 font-medium text-gray-800">{row.type_action}</td>
							<td class="px-4 py-2 text-gray-800">{row.description}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</section>

	<section class="space-y-4">
		<h2 class="text-xl font-semibold text-gray-900">Table 4&nbsp;: Priorité de l’action</h2>
		<div class="overflow-hidden rounded-lg border border-gray-200 bg-white shadow-sm">
			<table class="min-w-full text-sm">
				<thead class="bg-gray-50">
					<tr>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Échelle</th>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Définition</th>
						<th class="px-4 py-2 text-left font-semibold text-gray-700">Signification (score)</th>
					</tr>
				</thead>
				<tbody>
					{#each prioriteRows as row, i}
						<tr class={i % 2 === 0 ? 'bg-white' : 'bg-gray-50'}>
							<td class="px-4 py-2 font-medium text-gray-800">{row.echelle}</td>
							<td class="px-4 py-2 text-gray-800">{row.definition}</td>
							<td class="px-4 py-2 text-gray-800">{row.signification}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</section>
</main>

