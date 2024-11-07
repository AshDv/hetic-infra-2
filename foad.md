1. Composants AWS sélectionnés
Pour concevoir notre infrastructure sur AWS, nous avons sélectionné les composants suivants : Amazon CloudFront, Amazon S3, VPC, Elastic Load Balancer, AWS ECS/Fargate, AWS IAM, des sous-réseaux publics et privés. Chacun joue un rôle clé pour garantir que l’architecture soit sécurisée, performante et facilement scalable :
* Amazon CloudFront : Distribue les contenus de l’application (comme des fichiers et des images) rapidement et avec une faible latence grâce à un CDN (réseau de diffusion de contenu) mondial.
* Amazon S3 : Sert de stockage pour les fichiers statiques de l’application (images, fichiers JavaScript et CSS, etc.), offrant une grande durabilité et une haute disponibilité.
* VPC (Virtual Private Cloud) : Crée un réseau isolé pour nos ressources afin de mieux contrôler et sécuriser l’accès, en particulier pour protéger le backend de l’application.
* Elastic Load Balancer : Répartit le trafic de manière uniforme vers nos conteneurs ECS/Fargate pour assurer une haute disponibilité et faciliter l’ajout de ressources selon la demande.
* AWS ECS/Fargate : Exécute nos conteneurs Docker (backend et frontend) sans avoir besoin de gérer des serveurs, et facture seulement les ressources utilisées.
* AWS IAM : Gère les accès et les permissions pour sécuriser les ressources AWS, en s’assurant que seules les bonnes personnes et les bons services y ont accès.
* Sous-réseaux publics (public subnet) : Hébergent le Load Balancer et le point d’entrée de CloudFront, accessibles depuis l’extérieur.
* Sous-réseaux privés (private subnet) : Hébergent les tâches ECS, limitant ainsi l’accès direct à ces services critiques pour renforcer la sécurité.

2. Schéma de l'infrastructure

![Schema de l'infrastructure](/schema.png)

3. Justification des choix techniques
Nos choix de composants et de configuration visent à offrir une infrastructure performante, sécurisée, évolutive et facile à gérer :
* CloudFront et S3 : Ensemble, ils garantissent une distribution rapide et efficace des fichiers statiques en utilisant le cache de CloudFront.
* VPC et Sous-réseaux : Séparent les ressources critiques dans des sous-réseaux privés pour protéger le backend, tout en exposant seulement ce qui est nécessaire (comme le Load Balancer) dans un sous-réseau public.
* Elastic Load Balancer : Assure une répartition équilibrée du trafic et permet une mise à l’échelle automatique, ajoutant ou supprimant des conteneurs selon la demande.
* ECS/Fargate : Gère nos conteneurs Docker sans nécessiter d’infrastructure serveur, un gain de simplicité pour une application légère et performante.

4. Service AWS pour stocker et rendre les images Docker disponibles
Pour héberger et gérer nos images Docker (backend et frontend), nous utilisons Amazon Elastic Container Registry (ECR). ECR est un registre Docker sécurisé et entièrement géré, qui s’intègre facilement avec ECS et Fargate pour déployer les conteneurs. Cela nous permet de versionner les images, de gérer les permissions avec IAM, et d’automatiser les déploiements directement depuis ECR vers ECS.